# Shard your locks

https://strebkov.dev/posts/shard-your-locks/

Benchmarks six ways to guard a concurrent in-memory map in Go: plain mutex, `RWMutex`, `sync.Map`, a 256-shard map, and copy-on-write via `atomic.Pointer`. The verdict: default to sharded.

## When it's worth it

- Shared map with mixed reads and writes on multiple cores. A single mutex doesn't just plateau — it gets *slower* as cores grow (cache-line ping-pong on the lock). Sharding hit ~8x the single-mutex throughput at 8 cores.
- The technique is cheap: hash the key, pick a shard, each shard is its own map + mutex. ~15 lines of code. 256 shards was the sweet spot; more gives diminishing returns.

## When it's not

- Read-mostly data (config snapshots, feature flags, routing tables): copy-on-write with an atomic pointer is fully lock-free on reads and beats everything — but writes copy the whole map, so they must be rare.
- Low contention / few cores: a plain mutex is fine, don't bother.
- Hot-key skew (Zipfian): hot keys land on the same shard and contention comes back, eroding the gain.

## Surprises

- `RWMutex` is the wrong reflex for "read-heavy": the reader-counter bookkeeping makes it *worse* than a plain mutex once writes show up.
- Caveats of sharding: range/iterate operations need every shard locked, and shards must be padded to separate cache lines or false sharing eats the win.
