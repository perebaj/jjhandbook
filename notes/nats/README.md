Useful queries to investigate NATS server.

These commands return a summarized report of each consumer, with Unprocessed Messages, Consumption Mode, Ack Policy, and more.

kubectl -n nats-cluster exec deploy/nats-cluster-box -- nats consumer report <consumer-name>

╭───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│                                                              Consumer report for og-metrics with 5 consumers                                                              │
├──────────────────────┬──────┬────────────┬──────────┬─────────────┬─────────────┬──────────────┬────────────┬───────────┬─────────────────────────────────────────────────┤
│ Consumer             │ Mode │ Ack Policy │ Ack Wait │ Ack Pending │ Redelivered │ Unprocessed  │ Ack Floor  │ API Level │ Cluster                                         │
├──────────────────────┼──────┼────────────┼──────────┼─────────────┼─────────────┼──────────────┼────────────┼───────────┼─────────────────────────────────────────────────┤
│ xxxxx-detector       │ Pull │ Explicit   │ 30.00s   │ 193         │ 0           │ 2,067 / 3%   │ 40,545,716 │ 0         │ nats-cluster-0, nats-cluster-1, nats-cluster-2* │
│ xxx-metrics          │ Pull │ Explicit   │ 30.00s   │ 0           │ 546         │ 34,137 / 63% │ 40,511,207 │ 0         │ nats-cluster-0, nats-cluster-1, nats-cluster-2* │
│ xxxx-metrics         │ Pull │ Explicit   │ 30.00s   │ 0           │ 727         │ 35,685 / 66% │ 40,510,854 │ 0         │ nats-cluster-0, nats-cluster-1, nats-cluster-2* │
│ xxxxxxxxx-metrics    │ Pull │ Explicit   │ 30.00s   │ 521         │ 182         │ 34,571 / 64% │ 40,512,171 │ 0         │ nats-cluster-0, nats-cluster-1, nats-cluster-2* │
│ <consumer-naxxxxxme> │ Pull │ Explicit   │ 30.00s   │ 36          │ 449         │ 37,474 / 69% │ 40,509,894 │ 0         │ nats-cluster-0*, nats-cluster-1, nats-cluster-2 │
╰──────────────────────┴──────┴────────────┴──────────┴─────────────┴─────────────┴──────────────┴────────────┴───────────┴─────────────────────────────────────────────────╯

General information about the NATS stream

-- nats stream report

nats stream report <stream-name>
nats stream report <stream-name>

Specific information about a consumer

kubectl -n nats-cluster exec deploy/nats-cluster-box -- nats consumer info <stream-name> <consumer-name>