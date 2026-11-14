# A single hot key defeats a sharded lock

Notes from debugging a production log-storm detector. The service counts records per tenant in a sharded in-memory map. It looked healthy under load tests and fell over during a real storm. The problem was not CPU. It was a lock convoy.

## The notebook analogy

Picture counts kept in 16 notebooks. Each tenant maps to one fixed notebook by hash. To write, a worker must hold the notebook, and only one worker can hold it at a time.

On a normal day traffic spreads across tenants. 4 workers write in 4 different notebooks. Nobody waits.

During a storm millions of records come from ONE tenant. Now everyone needs the SAME notebook. 1 worker writes, 3 queue behind it. The service had 4 CPUs but only used 1.35. The workers were not idle. They were waiting in line.

It gets worse. Each worker grabbed the notebook, wrote ONE line, put it back, then grabbed it again for the next line. A message with 8000 records meant 8000 grab-and-release cycles, with three other workers fighting for the notebook every single time.

## The fix: batch under one lock

Workers jot a chunk of lines on their own private scratch pad first. No lock needed for that. Then they take the notebook ONCE and copy up to 256 lines in. That took us from 8000 lock round-trips per message to about 32.

Roughly:

```go
// before: lock per record
for _, rec := range msg.Records {
    shard := shardFor(rec.Key)
    shard.mu.Lock()
    shard.add(rec)
    shard.mu.Unlock()
}

// after: lock once per chunk
batch := batch[:0] // private, no lock
for _, rec := range msg.Records {
    batch = append(batch, rec)
    if len(batch) == 256 {
        flush(batch) // one Lock, copy all, Unlock
        batch = batch[:0]
    }
}
flush(batch)
```

## Lessons

### 1. Sharded locks only help when the keys spread

One hot key collapses N shards into 1 lock. And the usual benchmark hides this completely. If your benchmark spreads keys round-robin, it measures the best case. Write the hot-key benchmark too: every operation on the same key.

### 2. The smoking gun is anti-scaling

The same code got SLOWER as goroutines were added. On the hot key: 126ns/op at 1 core, 207ns/op at 4 cores. More workers, worse results. That is the fingerprint of a lock convoy, and a benchmark run with `-cpu 1,2,4` makes it visible without any profiler access.

After batching: flat 121-126ns/op across cores, and +64% throughput at 4 cores.

### 3. The production signature, so you recognize it next time

Three things together:

- input queue backlog grows
- the input buffer is full
- CPU sits far below the limit

Full buffer plus low CPU plus growing backlog means blocked, not starved. If the workers were starved for CPU, usage would be pinned at the limit. Ours sat at 1.35 of 4.

### 4. Batching has sharp edges with reused scratch buffers

The hot path was built on reused scratch buffers, which was fine when each record was processed and dropped immediately. Once records accumulate until a flush, each accumulated item must own its memory. Per-slot buffers fix that. Strings built over reused scratch must be copied, or the next record overwrites them before the flush reads them.

Catch the regression with an allocation benchmark. Our oversized-payload case went +793% on bytes per op until those payloads were made to bypass the batch.

### 5. Prove equivalence, not just speed

A test feeds identical input to the per-item path and the batched path, then asserts identical counts and identical outcome totals. Run it with the race detector on. A faster path that counts differently is just a bug with good benchmarks.

### 6. The end result

At 4 cores, on dense 500-record messages: -44% time and -91% allocated bytes.

## Related

- [Shard your locks](../../reading/shard-your-locks.md) covers when sharding wins and when hot-key skew erodes it. This note is the production version of that caveat.
