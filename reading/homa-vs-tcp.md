# Homa: The End of TCP for AI Clusters — John Ousterhout, Stanford

https://www.youtube.com/watch?v=eZ8WWZzoaR0

Homa on GitHub: https://github.com/PlatformLab/HomaModule

## References

- [Homa deep-dive by Harsh Kapadia](https://networking.harshkapadia.me/homa.html) — covers message vs packet, TCP's problems in datacenters, Homa packet types, and Linux kernel integration
- Papers:
  - [It's Time to Replace TCP in the Datacenter](https://arxiv.org/abs/2210.00714v2) — Ousterhout's position paper
  - [Homa: A Receiver-Driven Low-Latency Transport Protocol Using Network Priorities](https://arxiv.org/abs/1803.09615) — the original SIGCOMM paper
  - [A Linux Kernel Implementation of the Homa Transport Protocol](https://www.usenix.org/conference/atc21/presentation/ousterhout) — USENIX ATC '21
- [Homa protocol synopsis](https://github.com/PlatformLab/HomaModule/blob/master/protocol.md) — packet types and wire behavior
- Videos: [paper discussion](https://www.youtube.com/watch?v=nEFOni_87Yw) · [ATC '21 talk](https://www.youtube.com/watch?v=qu5WDcZRveo) · [Netdev 0x16 keynote](https://www.youtube.com/watch?v=o2HBHckrdQc)
- Micah Lerner's paper walkthrough: [part I](https://www.micahlerner.com/2021/08/15/a-linux-kernel-implementation-of-the-homa-transport-protocol.html) · [part II](https://www.micahlerner.com/2021/08/29/a-linux-kernel-implementation-of-the-homa-transport-protocol.html)

## Why this matters

AI workloads are shifting: training is still dominated by huge transfers (throughput-bound), but inference and agentic workloads exchange lots of small messages (KV-cache lookups, barrier sync, coordination metadata). For those, what matters is **tail latency** (p99), not throughput — one slow sync stalls every GPU waiting on it.

## Why TCP/RDMA fail here

- **Sender-side congestion control**: the sender only learns about congestion indirectly (drops or ECN marks echoed back by the receiver), with one bit of information and several round trips of lag. Systems never stabilize — they oscillate between sending too much and too little, and queues have to build up before anyone even notices congestion.
- **Byte-stream model**: TCP has no message boundaries. It can't know how much data is coming, can't prioritize short messages, and suffers head-of-line blocking — a small message serialized behind two big ones in the same stream just waits.
- **Incast**: many nodes sending to one destination overflow the top-of-rack switch egress queue; short messages get stuck behind long ones, and in the worst case drops + retransmissions make everything worse.

## What Homa does differently

1. **Message-based, not stream-based** — the fundamental unit is an RPC (request + response). Message length is known at the transport layer, so the receiver knows from packet one exactly how much more is coming. Messages are independent: short ones bypass long ones.
2. **Receiver-driven congestion control** — congestion happens at the last hop to the receiver, so the receiver manages it. Senders transmit only the first few packets unsolicited ("unscheduled"); the rest are sent on-demand via **grant packets** from the receiver, which paces grants to avoid queue buildup and favors short messages (SRPT — shortest remaining processing time first).
3. **Uses switch priority queues** — modern switches have ~8 priority queues per egress port; Homa assigns short messages to higher-priority queues so they bypass queued packets from long transfers.

## Results (from his benchmark)

- p99 for short messages: TCP >1 ms vs Homa <100 µs (~13x better).
- Long messages don't suffer — Homa is still ~2x better than TCP even on the largest messages (run-to-completion scheduling beats TCP's fair scheduling).

Ousterhout is semi-retired and working full time on Homa — Linux kernel module on GitHub, upstreaming in progress.
