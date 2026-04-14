# Scrape Memory Limiter — Research & Link Summaries

> Research document for [Prometheus Proposal #76](https://github.com/prometheus/proposals/pull/76) — Scrape Memory Limiter.
> Each link referenced in the proposal and its PR discussion is scraped and summarized below.

---

## Executive Summary

The Prometheus ecosystem has a long-standing problem with OOM crashes caused by scraping. When targets expose too many metrics or label churn grows unchecked, Prometheus memory usage spikes and the process is killed by the OOM killer. This creates a crash loop because the WAL replays the same data on restart, causing the same OOM.

Multiple approaches have been proposed over the years: limiting label churn (#17109), auto-recovering from OOM crash loops (#13939), forcing head compaction before scraping (#11306), rejecting only new series (#16917), and implementing a HEAD series hard limit (#11124). Each addresses a different facet of the problem, but none provides a holistic solution. The HEAD series limit PR (#11124) was rejected because it creates partial scrapes that violate Prometheus's transactionality guarantee — a fundamental design principle.

**Proposal #76 (Scrape Memory Limiter)** takes a different approach: skip entire scrapes when memory pressure is high, preserving transactionality. This is complementary to, not a replacement for, the other approaches. The OTel Collector already has a `memorylimiterprocessor` with a proven configuration model (`limit_mib`/`limit_percentage`, GOMEMLIMIT at 80%) that can inform the receiver-side implementation. Go's `runtime/metrics` package and `GOMEMLIMIT` provide the necessary APIs for memory tracking and GC tuning.

---

## Links Progress

| # | Link | Status |
|---|------|--------|
| 1 | prometheus/prometheus#17109 — Limit label churn / new series | DONE |
| 2 | prometheus/prometheus#13939 — Automated WAL deletion on OOM | DONE |
| 3 | prometheus/prometheus#11306 — Force head compaction before scraping | DONE |
| 4 | prometheus/prometheus#16917 — Rejecting only new series | DONE |
| 5 | prometheus/prometheus#11124 — PR attempt for rejecting new series | DONE |
| 6 | prometheus/prometheus#18284 — Related issue from dashpole | DONE |
| 7 | OTel memory limiter processor README | DONE |
| 8 | Go runtime GOMEMLIMIT docs | DONE |
| 9 | Scrape Trolley Dilemma talk | DONE |
| 10 | Consolidation / Executive Summary | DONE |

---

## Related Prometheus Issues

### 1. prometheus/prometheus#17109 — Scrape configuration to limit label churn / added series

- **Author:** @ringerc (Craig Ringer) | **Created:** 2025-09-01 | **State:** OPEN
- **Labels:** help wanted, priority/P2, component/scraping, kind/feature

**Problem:** There is no way to limit the number of new series a workload creates *over time*. The existing `sample_limit` only limits series per-scrape, not churn. A rogue workload rotating labels can grow the TSDB index massively.

**Two proposed approaches:**
1. **Threshold on `scrape_series_added`** with a first-scrape exception — reject scrapes when `scrape_series_added` exceeds a configured limit (e.g. `series_added_limit: 5`). New instances would get a grace period.
2. **Rolling window limit** — limit the sum of `scrape_series_added` over a time window (e.g. max 200 new series per hour via `series_added_over_time_limit`).

**Limitations acknowledged:**
- Go makes it hard to track per-job memory usage directly; series counts are an indirect proxy
- Requires the appender to query the TSDB, which has non-trivial cost
- Difficult to implement in agent mode (no TSDB)
- Won't defend against churn from rapidly restarting workloads

**Key discussion:** @bwplotka supports this and presented about it at PromCon 2025. @beorn7 suggested it deserves a formal proposal. @ringerc notes that Go's lack of per-object-graph memory tracking makes all limits crude proxies for what we really want to control (memory).

**Relevance to Scrape Memory Limiter:** Complementary. Churn limiting protects TSDB from slow cardinality leaks over time; the memory limiter protects the active heap from sudden bursts during scraping.

---

### 2. prometheus/prometheus#13939 — OOM crashloop auto-recovery

- **Author:** @bwplotka (Bartlomiej Plotka) | **Created:** 2024-04-16 | **State:** OPEN
- **Labels:** help wanted, kind/feature

**Problem:** When Prometheus OOMs, the WAL replays the same data on restart, causing the same OOM — a crash loop. Current recovery requires either (A) increasing memory limits temporarily or (B) manually deleting the WAL directory, losing ~2h of data.

**Recovery breakdown (manual process today):**
- B1: **Detection** — find the source of OOM (which target/metric/label)
- B2: **Limit** — apply sample limits, filtering, or disable the target
- B3: **Recover** — delete the problematic WAL data (often done first because there's no other way)

**Ideas discussed:**
- A "dangerous" flag to delete WAL files on every restart (for agent mode)
- Delete WAL only when OOM is detected (harder to implement)
- WAL backup mechanisms (e.g. rename before delete, upload to object storage)
- Memory usage threshold during WAL replay — abort replay and delete WAL if memory approaches the limit
- In-process vs sidecar debate: in-process is easier to adopt; sidecar works well with Prometheus Operator

**Google's approach:** At Google (GKE), they run `--storage.tsdb.delete-data-on-start` in their fork for ephemeral storage environments.

**Relevance to Scrape Memory Limiter:** Complementary. The memory limiter *prevents* OOM; this issue addresses *recovering* from OOM. Both are needed — the limiter reduces OOM frequency, but recovery is still needed when it happens.

---

### 3. prometheus/prometheus#11306 — Force head compaction/WAL truncation before starting scraping

- **Author:** @bboreham (Bryan Boreham) | **Created:** 2022-09-14 | **State:** OPEN
- **Labels:** priority/Pmaybe, component/tsdb, kind/feature

**Problem:** After an OOM crash, WAL replay consumes memory, then scraping starts and adds *more* data before the WAL is cleaned up, making things worse. This creates a cycle where each restart is worse than the last.

**Proposal:** Add a CLI flag `--force-head-compaction-at-start` that:
1. Replays the WAL
2. Compacts the HEAD and truncates the WAL *before* starting any scrapes
3. This frees memory from old data before new scrapes begin

**Key insight from @bboreham:** It's critical to NOT start scraping until after compaction finishes, otherwise memory usage just grows and the WAL gets bigger, making the next restart worse.

**Relevance to Scrape Memory Limiter:** Complementary. This addresses the startup-specific OOM scenario; the memory limiter addresses the steady-state scraping scenario. Both prevent different OOM patterns.

---

### 4. prometheus/prometheus#16917 — Handle OOMs gracefully by rejecting scrapes adding new series

- **Author:** @codesome (Ganesh Vernekar) | **Created:** 2025-07-23 | **State:** OPEN
- **Labels:** kind/enhancement, kind/more-info-needed, component/scraping, component/tsdb

**Problem:** Same as the broader OOM problem. Instead of halting scrapes entirely, reject scrapes that try to add new series while continuing to accept updates for existing series.

**Key design constraint:** "partial scrapes is not okay, so we must reject entire scrape" — meaning if a scrape would create new series and we're over the threshold, the *entire* scrape is dropped.

**Discussion:**
- @beorn7: Measuring memory consumption is hard in Go; needs a design doc before implementation
- @prymitive: References PR #11124 which had a HEAD limit but was rejected; asks if there's been a change of heart
- @beorn7: #11124 was rejected because it caused partial scrapes; this issue explicitly avoids that

**Relevance to Scrape Memory Limiter:** This is essentially a precursor to Proposal #76. The proposal formalizes and expands on this idea with proper configuration, GOMEMLIMIT interaction, and an experimental feature flag.

---

## Alternative Approaches (Rejected/Closed)

### 5. prometheus/prometheus#11124 — Head series limit (PR, CLOSED)

- **Author:** @prymitive (Lukasz Mierzwa) | **Created:** 2022-08-05 | **State:** CLOSED (not merged)

**What it did:** Added a global HEAD series limit to TSDB. When the limit was reached, appends that would create *new* series were rejected, but appends updating *existing* series continued. This created a "soft limit" — Prometheus kept running in a degraded state instead of crashing.

**Why it was rejected:**
- **@roidelapluie:** "scrapes should be ingested in full or not ingested at all" — strongly disagreed with partial ingestion
- **@SuperQ:** Partial ingestion breaks fundamental system behavior assumptions. Example: if `http_requests_total{status="5xx"}` is dropped but `{status="2xx"}` is kept, success rate queries silently produce wrong results. This violates the "principle of least surprise."
- Even metric-family-level granularity doesn't help: `some_errors_total` could be dropped while `some_actions_total` is kept, still skewing ratios.

**Author's counterargument:** @prymitive ran this in production for years and it prevented many OOMs. Also pointed out that metric relabel rules already allow arbitrary series dropping (same failure mode). @SuperQ called this "whataboutism."

**Key lesson for Scrape Memory Limiter:** The Prometheus community has a **strong consensus against partial scrapes**. Any memory limiting mechanism MUST drop entire scrapes, not individual series. Proposal #76 respects this by skipping the entire scrape when memory is over the limit.

---

### 6. prometheus/prometheus#18284 — Consider adding built-in metric for scrape failure reason

- **Author:** @bwplotka | **Created:** 2026-03-12 | **State:** OPEN

**What it proposes:** Add a built-in scrape metric with limited cardinality that categorizes why scrapes fail: `scrapes_failed_total{area=[client / network / timeout / scrape_limit / memory_limit / storage]}`. Currently there is no metric for scrape failures in Prometheus.

**Motivation:** Driven by Proposal #76 — adding a memory limiter introduces another non-trivial reason for scrape failure, and users need visibility into *why* scrapes fail, not just that they failed.

**Discussion:**
- @dashpole suggests gating it behind `extra_scrape_metrics` config to avoid increasing default cardinality
- No PRs yet — consensus needed first

**Relevance to Scrape Memory Limiter:** Directly related. This would be the debuggability metric that operators use to understand memory limiter impact. The OTel receiver should expose equivalent telemetry (e.g. `prometheus_target_scrapes_skipped_memory_limit_total`).

---

## Reference Material

### 7. OTel Collector Memory Limiter Processor

- **Source:** [memorylimiterprocessor README](https://github.com/open-telemetry/opentelemetry-collector/blob/main/processor/memorylimiterprocessor/README.md)

**What it is:** A processor that prevents OOM by monitoring heap usage and refusing data when thresholds are exceeded. Returns errors to upstream components, triggering retries and backpressure.

**How it works:**
1. Periodically checks memory (configurable `check_interval`, recommended 1s)
2. Maintains two limits: **hard limit** (`limit_mib`) and **soft limit** (hard limit minus `spike_limit_mib`)
3. When memory exceeds soft limit → enters "memory limited mode", returns errors upstream
4. When memory exceeds hard limit → forces garbage collection
5. Resumes normal operation when memory drops below soft limit

**Configuration:**

| Option | Purpose | Default |
|--------|---------|---------|
| `check_interval` | Frequency of memory measurements | 0s (1s recommended) |
| `limit_mib` | Hard limit in MiB | 0 (must set this or `limit_percentage`) |
| `spike_limit_mib` | Expected memory spike between checks | 20% of `limit_mib` |
| `limit_percentage` | Hard limit as % of system memory | 0 |
| `spike_limit_percentage` | Spike as % of system memory | 0 |

**Best practices:**
1. **Set `GOMEMLIMIT` to 80% of the hard memory limit** — ensures GC kicks in before data is dropped
2. Position as first processor in pipelines for maximum backpressure effectiveness
3. Size `spike_limit_mib` to prevent exceeding hard limits between checks (start at 20%)
4. Use `limit_percentage` for containers; `limit_mib` for bare metal

**Relevance:** The Prometheus receiver's memory limiter should follow this proven model. The configuration shape (`limit_mib`/`limit_percentage`) and the GOMEMLIMIT 80% recommendation are directly applicable.

---

### 8. Go Runtime — GOMEMLIMIT and Memory APIs

- **Source:** [pkg.go.dev/runtime](https://pkg.go.dev/runtime)

**GOMEMLIMIT:**
- Sets a soft memory limit for the Go runtime
- When approached, GC becomes more aggressive to keep memory below the threshold
- Default: `math.MaxInt64` (effectively disabled)
- Syntax: `GOMEMLIMIT=512MiB` (supports B, KiB, MiB, GiB, TiB)
- Includes Go heap + runtime-managed memory; excludes external memory (C, OS, mmap)
- Can be set at runtime via `runtime/debug.SetMemoryLimit()`
- Soft enforcement: runtime *tries* to stay under the limit but may temporarily exceed it

**APIs for reading memory:**

1. **`runtime.ReadMemStats()`** — classic approach, provides `Alloc`, `Sys`, `HeapAlloc`, `HeapInuse`, `NumGC` etc.
2. **`runtime/debug.SetMemoryLimit()`** — get/set GOMEMLIMIT programmatically (`-1` to read without changing)
3. **`runtime/metrics`** (Go 1.16+) — modern approach with structured metrics like `/memory/allocs:bytes`, `/gc/heap/goal:bytes`, `/memory/live:bytes`

**Relevance:** The proposal suggests using `runtime/metrics` for memory checking. The `GOMEMLIMIT` interaction is key — the scrape memory limiter's limit should be *higher* than GOMEMLIMIT so GC is more aggressive before scrapes are dropped.

---

### 9. Scrape Trolley Dilemma Talk (PromCon 2025)

- **Speaker:** @bwplotka (Bartlomiej Plotka)
- **Video:** [YouTube](https://www.youtube.com/watch?v=ulHQUCarjjo)
- **Slides:** [Google Slides](https://docs.google.com/presentation/d/1jKrUklPdAor9292HrPWtJkIa6ruUhOGo9IFO7fNj-DE/edit)

**Context:** Lightning talk at PromCon 2025 that motivated the entire Scrape Memory Limiter proposal. The "Trolley Problem" analogy: when one rogue target causes Prometheus to OOM, it takes down monitoring for ALL targets. The question is whether to sacrifice monitoring of the rogue target to save everyone else.

**Key arguments (from issue discussions referencing the talk):**
- Dynamic service discovery (e.g. Kubernetes) makes it impossible to predict target sizes ahead of time
- Static `sample_limit` is insufficient — requires prior knowledge and per-job tuning
- OS-level memory limits (container limits) cause hard crashes with no graceful degradation
- Even random but stable choice of which scrapes to skip is better than killing everything
- The talk drove consensus among maintainers to pursue a formal proposal

**Relevance:** This is the origin story of Proposal #76. The talk built community consensus that proactive memory limiting is needed and acceptable, overcoming previous resistance (cf. PR #11124's rejection).

---

## Key Takeaways for Implementation

1. **Transactionality is non-negotiable:** The Prometheus community will reject any approach that creates partial scrapes. The memory limiter MUST skip entire scrapes or accept them in full.

2. **Proven OTel model exists:** The `memorylimiterprocessor` provides a battle-tested configuration model and best practices (GOMEMLIMIT at 80%, check_interval of 1s, spike_limit concept).

3. **GOMEMLIMIT interaction is critical:** GOMEMLIMIT should be lower than the scrape memory limit so GC runs aggressively before scrapes are dropped. Proposal #76 auto-configures this.

4. **Multiple complementary solutions needed:** The memory limiter prevents OOM during scraping, but the ecosystem also needs: churn limiting (#17109), crash loop recovery (#13939), startup compaction (#11306), and failure reason metrics (#18284).

5. **`runtime/metrics` is the preferred API:** Use Go's `runtime/metrics` package for memory checking rather than the older `runtime.ReadMemStats()`.

6. **Debuggability is a first-class concern:** Users need to know WHY scrapes fail. The `up=0` metric, `/targets` page error, and a counter metric for skipped scrapes are all required.

7. **Feature flag gating:** Start experimental, behind a feature flag. Graduate based on production feedback.

## Open Questions

1. Should the OTel receiver reuse the Prometheus-native memory limiter config (passthrough to `scrape.Manager`) or implement its own using the OTel `memorylimiterprocessor` model?
2. How does the memory limiter interact with the OTel Collector's existing `memorylimiterprocessor` when both are configured?
3. Should the receiver support the "soft limit" / spike_limit concept from the OTel processor, or start simple with just a hard limit?
4. What is the right default for `check_interval`? The OTel processor recommends 1s; the proposal also suggests ~1s.
5. How to handle the case where the Prometheus receiver runs inside a collector that already has GOMEMLIMIT set by the operator?
