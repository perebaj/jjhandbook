# Postgres: row versions, VACUUM, and how write churn hurts reads

The short version: in Postgres an `UPDATE` never changes a row in place, and a
`DELETE` never removes one. Both leave the old row version behind. A background
process, autovacuum, cleans these up later. When a table is written faster than
autovacuum keeps up, the leftovers make reads slow. That includes reads that
look like they never touch the table, such as index-only scans.

I learned this chasing a slow `count(*) ... GROUP BY` over the "active" rows of
a table that was mostly history. The query was slow because of how the table
was **written**, not because of the query itself.

## Layer 1: every write creates a row version (MVCC)

MVCC, or Multi-Version Concurrency Control, is how Postgres lets readers and
writers run without blocking each other. Each transaction sees a consistent
snapshot, so the database has to keep old row versions around for as long as
someone might still need them.

- A row version is called a **tuple**. Each tuple carries two hidden
  transaction IDs:
  - `xmin` is the transaction that created it.
  - `xmax` is the transaction that deleted or replaced it. It is empty while
    the row is live.
- `INSERT` writes a new tuple with `xmin` set.
- `DELETE` sets `xmax` on the existing tuple. The bytes stay on disk.
- `UPDATE` is a `DELETE` plus an `INSERT`: it sets `xmax` on the old tuple and
  writes a complete new tuple, usually in another place.
- A tuple whose `xmax` committed before every open snapshot is **dead**: no one
  can see it anymore, but it still takes up space until it is vacuumed.

So a table that is "small" in live rows can be large on disk. The common
pattern "archive the old row, insert a new one" produces one dead tuple and
one new tuple per cycle, forever.

```sql
-- See the hidden columns
SELECT xmin, xmax, ctid, * FROM my_table LIMIT 5;
```

`ctid` is the tuple's physical location (page, slot). It changes on every
non-HOT update, which shows the row really moved.

## Layer 2: indexes pay for every new version too

An index entry points at a tuple's `ctid`. A new tuple means new index entries
**in every index of the table**, including indexes on columns that didn't
change. With 30 indexes, one `UPDATE` is about 31 writes, plus WAL for each
one. Dead index entries also stay until vacuum.

The exception is a **HOT update** (Heap-Only Tuple). If no indexed column
changed **and** the new version fits on the same page, Postgres chains the new
tuple from the old one and skips the index writes entirely.

- An update that flips an indexed column, for example `status = 'archived'`
  with `status` in several indexes, can never be HOT.
- `fillfactor` below 100 leaves free space on each page, so HOT has room to
  work: `ALTER TABLE t SET (fillfactor = 90)`.
- `n_tup_hot_upd = 0` next to a large `n_tup_upd` in `pg_stat_user_tables` is
  the red flag: every update is paying the full index cost.

Prefer updates that touch only non-indexed columns, such as a `last_seen_at`
that no index covers. Every extra index you add makes every update more
expensive.

## Layer 3: the visibility map and why index-only scans still read the table

An index doesn't know whether the tuple it points at is visible to your
snapshot. Visibility lives in the table (the **heap**), in `xmin` and `xmax`.

The **visibility map** (VM) is a small bitmap with bits per heap page, and the
one that matters here is **all-visible**: "every tuple on this page is visible
to everyone". VACUUM sets the bit, and any write to the page clears it.

- **Index Only Scan** reads only the index when the page is all-visible.
- When the bit is clear, it must fetch the heap page to check visibility. That
  shows up as `Heap Fetches` in `EXPLAIN ANALYZE`.

```
Index Only Scan using idx_x on t  (actual rows=77,599)
  Heap Fetches: 99,584      ← the "index-only" scan read the table ~100k times
```

So on a churned table an index-only plan quietly turns into random heap I/O.
The fix is not a new index. The fix is to keep the VM fresh, which means
vacuuming more often, and to write less.

```sql
-- Share of the table that is all-visible (no extension needed)
SELECT relname, relpages, relallvisible,
       round(100.0 * relallvisible / nullif(relpages, 0), 1) AS pct_all_visible
FROM pg_class WHERE relname = 'my_table';
```

## Layer 4: VACUUM, the cleanup protocol

`VACUUM` (plain, not `FULL`) does this:

1. Removes dead tuples from the heap and their entries from every index.
2. Records the freed space in the **free space map** (FSM), so new tuples reuse
   it.
3. Sets all-visible bits in the VM.
4. **Freezes** old tuples, marking them visible forever, so the 32-bit
   transaction ID counter can wrap around safely.

It does **not** shrink the file. Freed space is reused, not returned to the
OS. Shrinking takes `VACUUM FULL`, which rewrites the table under an exclusive
lock, or `pg_repack`, which does it online.

Plain `VACUUM` doesn't block reads or writes, but it does use I/O. Running it
by hand is the quickest experiment to prove a theory:

```sql
VACUUM (VERBOSE) my_table;   -- then rerun EXPLAIN (ANALYZE, BUFFERS)
```

If `Heap Fetches` drops to near zero and the query gets fast, the problem was
vacuum lag.

### Autovacuum and when it fires

Autovacuum workers vacuum a table when its dead tuples pass a threshold:

```
vacuum when  n_dead_tup > autovacuum_vacuum_threshold
                          + autovacuum_vacuum_scale_factor * reltuples
```

The defaults are `50 + 0.2 * reltuples`: 20% of the table must be dead. On a
7M-row table that is about 1.4M dead tuples before anything happens. On big,
hot tables that is far too late.

Since PG13 there is also an **insert** trigger,
`autovacuum_vacuum_insert_threshold` and
`autovacuum_vacuum_insert_scale_factor`. It vacuums insert-heavy tables so the
VM gets set and old tuples get frozen.

Tune it per table, not globally:

```sql
ALTER TABLE my_table SET (
  autovacuum_vacuum_scale_factor = 0.01,   -- 1% instead of 20%
  autovacuum_vacuum_threshold    = 10000,
  autovacuum_analyze_scale_factor = 0.01,
  autovacuum_vacuum_cost_limit   = 2000    -- let it work faster
);
```

Other knobs:

- `autovacuum_naptime` sets how often the launcher checks (default 1min).
- `autovacuum_max_workers` sets how many tables are vacuumed at once.
- `maintenance_work_mem` caps the memory each vacuum uses to track dead tuples
  (`autovacuum_work_mem`, if set, overrides it). Too small means multiple
  passes over the indexes.
- `autovacuum_vacuum_cost_delay` and `autovacuum_vacuum_cost_limit` throttle
  vacuum I/O. The defaults are gentle, which is good for a quiet table and bad
  for a hot one.

### What stops VACUUM from cleaning

VACUUM can only remove tuples that are dead to **every** snapshot. One old
snapshot holds back the "xmin horizon" for the whole cluster:

- long-running transactions, including `idle in transaction` sessions;
- abandoned replication slots;
- `hot_standby_feedback = on` with long queries on a replica.

```sql
SELECT pid, state, now() - xact_start AS xact_age, query
FROM pg_stat_activity
WHERE xact_start IS NOT NULL
ORDER BY xact_age DESC LIMIT 10;
```

## Layer 5: ANALYZE and stale statistics

Autovacuum also runs **ANALYZE**, which samples the table and stores column
statistics (histograms, most-common values) that the planner uses to estimate
row counts. Its threshold works the same way, through
`autovacuum_analyze_scale_factor`.

Stale stats bite hardest on "recent rows" queries. Take
`WHERE created_at > now() - interval '1 day' ORDER BY created_at DESC LIMIT 1`:

- If the histogram's max `created_at` is from yesterday, the planner thinks
  almost no rows match. It may pick an index that walks `created_at` and then
  filters out most of what it reads, instead of the selective composite index.
  In the case I traced, that was 29,000 buffers and 47 ms instead of 7
  buffers and 1 ms.
- Postgres can probe the real current max at plan time, but only through an
  index whose **leading** column is that column. A partial index or a
  composite with another leading column doesn't help.
- The quick fix is `ANALYZE my_table`. The lasting fix is a lower
  `autovacuum_analyze_scale_factor` on that table, or a plain index that leads
  on the time column.

## How it all hurts: the chain

```
write churn (UPDATE/DELETE, archive + reinsert)
  → dead tuples in the heap and in every index        (bloat)
  → visibility map bits cleared                       (no all-visible pages)
  → index-only scans do heap fetches                  (random I/O)
  → more buffers per query, more cache pressure       (everyone gets slower)
  → autovacuum, tuned for small tables, arrives late  (the loop feeds itself)
```

Side effects to remember:

- **Bloat** makes tables and indexes bigger than their live data. Scans read
  more pages, and the cache holds fewer useful rows.
- **Write amplification:** N indexes × non-HOT updates, plus WAL, which also
  means replication lag and backup size.
- **Wraparound:** if freezing falls far enough behind, Postgres forces an
  aggressive anti-wraparound vacuum and, at the extreme, stops accepting
  writes.
- **Stale stats** lead to bad plans on exactly the hot queries.

## How to design around it

- **Write less.** Replacing a live row by archive-plus-insert creates churn.
  Updating one non-indexed column in place (`last_seen_at`) can be HOT and
  costs almost nothing.
- **Fewer indexes.** Every index is paid on every non-HOT write. Drop unused
  and prefix-redundant ones (see the query below).
- **Partial indexes for the hot subset**, such as `WHERE status = 'active'`.
  They are small, and rows that leave the subset stop costing.
- **Split hot from cold.** If 99% of the table is history and the queries only
  want the live 1%, keep the live rows in their own table or partition. The
  live set then fits in memory and vacuums in seconds.
- **Per-table autovacuum settings** sized to the write rate, not the defaults.
- **Prove before you redesign:** a manual `VACUUM` plus the same
  `EXPLAIN (ANALYZE, BUFFERS)` tells you in minutes whether vacuum lag is the
  cause.

## Queries I keep coming back to

```sql
-- Dead tuples, last (auto)vacuum/analyze, HOT ratio
SELECT relname, n_live_tup, n_dead_tup,
       round(100.0 * n_dead_tup / nullif(n_live_tup, 0), 1) AS dead_pct,
       n_tup_upd, n_tup_hot_upd, last_autovacuum, last_autoanalyze
FROM pg_stat_user_tables ORDER BY n_dead_tup DESC LIMIT 10;

-- When will autovacuum fire for this table? (uses per-table reloptions if set)
SELECT c.relname, c.reltuples::bigint, s.n_dead_tup, c.reloptions,
       current_setting('autovacuum_vacuum_threshold')::int
       + current_setting('autovacuum_vacuum_scale_factor')::float * c.reltuples AS default_trigger
FROM pg_class c JOIN pg_stat_user_tables s ON s.relid = c.oid
WHERE c.relname = 'my_table';

-- Heap vs index size
SELECT pg_size_pretty(pg_relation_size('my_table'))  AS heap,
       pg_size_pretty(pg_indexes_size('my_table'))   AS indexes;

-- Unused or barely used indexes (counters since the last stats reset)
SELECT indexrelname, idx_scan, pg_size_pretty(pg_relation_size(indexrelid)) AS size
FROM pg_stat_user_indexes WHERE relname = 'my_table' ORDER BY idx_scan;

-- What the planner thinks the newest value is
SELECT histogram_bounds FROM pg_stats
WHERE tablename = 'my_table' AND attname = 'created_at';
```

For plans, use `EXPLAIN (ANALYZE, BUFFERS)`. Watch `Heap Fetches`,
`Rows Removed by Filter` and `shared hit`/`read`, not just the time.

## Glossary

- **MVCC**: Multi-Version Concurrency Control. Writers create new row versions
  instead of overwriting, so readers keep a consistent snapshot without locks.
- **Tuple**: one physical version of a row.
- **xmin / xmax**: the transaction IDs that created and that deleted or
  replaced a tuple.
- **Dead tuple**: a version no open snapshot can see. It waits for VACUUM.
- **Heap**: the table's main data file, as opposed to its indexes.
- **HOT update**: Heap-Only Tuple. An update that changes no indexed column and
  fits on the same page, so it skips all index writes.
- **fillfactor**: the share of each page filled on insert. Below 100 leaves
  room for HOT updates.
- **Visibility map (VM)**: per-page bits; all-visible lets index-only scans
  skip the heap.
- **Heap Fetches**: in `EXPLAIN`, the heap reads an index-only scan still had
  to do.
- **Free space map (FSM)**: where VACUUM records reusable space.
- **Freezing**: marking old tuples as visible forever, to survive transaction
  ID wraparound.
- **xmin horizon**: the oldest snapshot still needed. Nothing newer than it can
  be cleaned.
- **Bloat**: space taken by dead tuples and entries, beyond the live data.
- **ANALYZE**: samples a table to refresh the planner's statistics.

## Further reading

- PostgreSQL docs, [Routine Vacuuming](https://www.postgresql.org/docs/current/routine-vacuuming.html)
- PostgreSQL docs, [Heap-Only Tuples](https://www.postgresql.org/docs/current/storage-hot.html)
- PostgreSQL docs, [Index-Only Scans and Covering Indexes](https://www.postgresql.org/docs/current/indexes-index-only-scans.html)
- PostgreSQL docs, [Visibility Map](https://www.postgresql.org/docs/current/storage-vm.html)
