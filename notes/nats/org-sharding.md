# Scaling stateful NATS consumers with org sharding

How to scale message consumption in JetStream when the consumer keeps state in memory. Motivated by a real case: a log-storm detector running as a single replica, holding sliding-window counters per tenant, and the question "can we just add replicas?". The answer is no, and the why is worth writing down.

## The problem shape

A JetStream durable pull consumer has work-queue semantics. Every pod that binds the same durable gets a disjoint subset of the messages. That is exactly what you want for stateless workers. It is silently wrong for stateful ones.

If the detection or aggregation state is process-local (sliding-window counters, fingerprint maps), two pods splitting one tenant's messages each see partial counts. Thresholds are never met. Detections go missing. Nothing crashes, nothing errors, the pods look healthy. You get false negatives, not failures.

So "just add replicas" corrupts correctness invisibly. That is the worst kind of bug, because every dashboard stays green.

## Why keep state process-local at all

The obvious fix is to move the state somewhere shared, like NATS KV or Redis. But shared state in the hot path costs a network round trip per update and creates its own contention. For high-volume counting, that is a real tax.

A design that accepts single-writer-per-tenant keeps the counting exact, in memory, and lock-free across the network. You trade "any pod can process anything" for cheap and correct state. The scaling question then stops being about state and becomes about routing.

## The invariant

Every message of a given tenant must land on exactly one active pod. Two ways to get that in NATS.

### Non-overlapping FilterSubjects per shard

If the subject already carries the tenant token, like `app.<tenant>.in.logs`, you can create N durable consumers. Each one filters the subject set that hashes to its shard. Simple to reason about. The downside is maintenance: the per-consumer subject list has to be kept in sync as tenants come and go, or you need a subject scheme with a wildcard per partition.

### Deterministic subject mapping with partition()

NATS has native subject transforms with a `partition` function. The server maps the original subject to a partitioned one, deterministically, based on a hash of the tokens you pick:

```
app.*.in.logs -> app.{{partition(4,1)}}.{{wildcard(1)}}.in.logs
```

Now `app.acme.in.logs` becomes something like `app.2.acme.in.logs`, and the same tenant always hashes to the same partition. Each shard consumer filters one partition, like `app.2.*.in.logs`. The mapping lives server-side, is deterministic, and survives restarts. No per-tenant list to maintain.

## Pinning pods to shards

The consumer fleet runs as a StatefulSet where the pod ordinal is the shard number. Pod 0 binds `detector-shard-0`, pod 1 binds `detector-shard-1`, and so on. Each process owns its shard's windows exclusively, so the one-process-owns-one-window invariant holds per shard.

Contrast with the naive setup: a Deployment where every replica binds one shared durable. That is the broken case from the problem shape above. Same number of pods, completely different correctness.

## Resharding

Changing N (the shard count) moves tenants between shards. If the windowed state is intentionally ephemeral, meaning it is not restored on restart, this costs only a brief undercount while the windows refill. That is the same trade a `Recreate` deploy strategy already makes on every rollout.

This is a reason to design the state as ephemeral in the first place. Durable windowed state would turn every reshard into a migration project. Ephemeral state turns it into a config change.

For dedup across a reshard, the publish side can set `Nats-Msg-Id` and let JetStream's dedup window backstop duplicates. The consumer does not have to handle that case.

## Guard the invariant

Nothing in Kubernetes stops `kubectl scale` from breaking the single-writer assumption. And the failure is silent, so you will not notice from the outside.

Options, from best to weakest:

- The process verifies at startup that its shard assignment is exclusive. Consumer info shows other active pulls, so a pod can refuse to start if someone else is already consuming its shard.
- Alert on overlapping consumers, watching consumer info from the outside.
- Manifests-only enforcement, `replicas: 1` plus `Recreate`. It works, but it is one manual command away from silent corruption. Fine as a first step, not as the end state.

## Before sharding, check the bottleneck

One closing observation on sequencing. Before sharding horizontally, check whether the bottleneck is really total volume.

Dense messages, say thousands of records packed into one message, create serial per-message work. More pods do not fix that, because the unit of parallelism is the message. Capping records per published message at the producer parallelizes the work inside a single pod, no sharding needed.

Shard when total tenant volume outgrows one machine. Not when message density does.
