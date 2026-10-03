# Checklist: manage distributed systems like a pro

Each item is a headline to be detailed later. The theme behind all of them: **enforce limits at the layer that owns the resource, and assume every client misbehaves.**

## Resource governance

- [ ] **Server-side limits per user/tenant** — memory, execution time, rows read/returned. The app asks nicely; the server enforces.
- [ ] **Quotas per tenant** — concurrent requests and requests-per-interval, so one noisy tenant can't starve the rest.
- [ ] **Workload isolation** — ingest, interactive reads, and background jobs get separate users/profiles/priorities, never one shared identity.
- [ ] **Memory ceiling below the kill threshold** — the service must hit its own limit (graceful error) before the kernel/k8s OOMKills it (restart loop).
- [ ] **Disk headroom rule** — define the max steady-state usage (~70%) and the expansion runbook *before* the 90% alert fires.

## Connections & flow

- [ ] **Connection pooling with a ceiling** — every direct client carries its own bounded pool; a gateway pool only covers the clients behind it. The server-side per-user quota is the global budget, and the sum of budgets must fit the server's capacity.
- [ ] **Timeout hierarchy** — client timeout < gateway timeout < server kill timeout, with cancellation propagated; otherwise abandoned work keeps burning resources.
- [ ] **Queue or fail-fast, decided on purpose** — excess load either waits in a short bounded queue or errors clearly; never an implicit infinite queue.
- [ ] **Backoff with jitter on reconnect** — mass reconnection after a restart is a self-inflicted DDoS (thundering herd).
- [ ] **Batching on the write path** — few large writes beat many small ones; enforce it in the contract or buffer server-side.

## Data lifecycle

- [ ] **Retention defined at table creation** — TTLs on product data, not just system tables; retrofitting retention on terabytes is an incident.
- [ ] **Tenant-aware physical layout** — partition by time, order/cluster by tenant; never partition by tenant.
- [ ] **Tiered storage as the middle step** — hot local / cold object storage buys years before sharding.

## Failure & recovery

- [ ] **RPO/RTO written down** — how much data you accept losing, how long you accept being down; everything else derives from these two numbers.
- [ ] **Backups are only real after a timed restore** — drill periodically, measure duration at realistic volume, not once at 100 GiB.
- [ ] **Incremental backup chain** — full backups stop scaling long before the data does.
- [ ] **Failure drills before prod** — kill the pod, drain the node, fill the disk; record what survives and write the runbook from what you saw.
- [ ] **HA costed honestly** — compare alternatives at the replication level the SLA requires, not at the single-node price.

## Operations

- [ ] **Alerts page on symptoms, limits prevent damage** — an alert is the notification that a protection worked (or was missing), never the protection itself.
- [ ] **Metrics allow-list** — observability has a cost model too; unbounded cardinality contaminates the thing you're measuring.
- [ ] **Load-test concurrency, not just volume** — "how many simultaneous users per node" is the capacity-planning number; derive a scaling rule from it.
- [ ] **Upgrades rehearsed in a lower environment** — same mechanism (GitOps), same data shape, before prod ever sees the version.
