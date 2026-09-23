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

## How big is the change? A layer-by-layer view

Surprisingly small on the network side — all the cost lives at the endpoints.

What does NOT change:

- **Physical + link (Ethernet)** — same cables, NICs, frames.
- **Switches** — untouched, and this is the clever part of the design: the ~8 priority queues per egress port that Homa uses already exist in any modern datacenter switch; Homa just marks packets (DSCP/priority field) to pick the queue. No new firmware. Contrast with RDMA/RoCE, which in practice demands PFC configured across the whole fabric and is notoriously fragile because of it.
- **IP (network layer)** — Homa is a transport riding inside IP packets exactly like TCP/UDP; routing and ECMP work unchanged.

What DOES change:

- **Transport (L4)** — fully replaced: a Linux kernel module registering a new protocol next to TCP/UDP, needed on **both ends** (Homa doesn't interop with TCP). Hence the upstreaming effort — until it's in mainline Linux, every host in the cluster needs the module.
- **API / application** — the dominant practical cost. The interface goes from stream sockets (`connect`/`read`/`write`) to message/RPC sockets, so existing code doesn't migrate for free. Mitigation: swap the transport one layer above the app, inside RPC frameworks — a gRPC integration (C++ and Java) exists from Ousterhout's group, turning the migration into framework config instead of a rewrite.
- **Ecosystem** — the hidden cost: L4 load balancers, firewalls, middleboxes, kube-proxy, service meshes, network metrics — anything that inspects or intermediates TCP connections needs to learn the new protocol or get out of the way.

As physical infrastructure change: near zero. As software change: a kernel module per host plus adoption via RPC framework, doable incrementally (each RPC is independent). As ecosystem change: large and slow — the same reason QUIC needed Google's weight and still disguised itself as UDP to cross middleboxes. Homa dodges part of that by targeting only the datacenter, an environment you control end to end — it explicitly does **not** aim at the open internet (assumes low RTTs, reliable network, no hostile middleboxes).

## Results (from his benchmark)

- p99 for short messages: TCP >1 ms vs Homa <100 µs (~13x better).
- Long messages don't suffer — Homa is still ~2x better than TCP even on the largest messages (run-to-completion scheduling beats TCP's fair scheduling).

Ousterhout is semi-retired and working full time on Homa — Linux kernel module on GitHub, upstreaming in progress.
