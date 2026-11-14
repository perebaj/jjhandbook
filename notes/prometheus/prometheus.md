* [Instrumentation](#instrumentation---three-types-of-services)
    * [online-serving](#online-serving)
    * [offline-processing](#offline-processing)
    * [batch-jobs](#batch-jobs)
* [Aggregation](#aggregation)
    * [Sum x Count](#sum-x-count)
    * [Aggregation and grouping operators](#aggregation-and-grouping-operators)
* [Metric Type](#metrics-type)
    * [Counter](#counter)
    * [Gauge](#gauge)
    * [Histogram](#histogram)
* [Useful Queries](#useful-queries)






# Instrumentation - Three types of services

[Instrumentation | Prometheus](https://prometheus.io/docs/practices/instrumentation/#instrumentation)

## Online Serving

Types of service that expect an immediate response. For example, a request to a database.

The main metrics in such a system are the number of queries performed, errors and latency. The number of in-progress requests can also be useful.

## Offline-processing

Type of system where you don't actively wait for its response.
For each stage it is important to track the items that arrive, how many are in progress, the last time you processed something, or even what the ratio is between items going out/coming in.

## Batch Jobs

They have a peculiar characteristic: they are not types of systems that run frequently, which makes collecting their metrics a bit more difficult.

A key metric for *batch* services is the last time it ran successfully.

How long each step took

# Aggregation

## Sum x Count


The main difference between the two functions is that "sum" returns the sum of the metric's values, while "count" returns the number of samples of the metric. In other words, "sum" is an aggregation function that deals with numeric values, while "count" is an aggregation function that deals with counting samples.

--- 

## Aggregation and grouping operators
Aggregation operators can be used

These operators can be used to aggregate Prometheus metrics across all label dimensions or to preserve distinct dimensions by including a "without" or "by" clause. These clauses can be used before or after the expression.

```sql
<aggr-op> [without|by (<label list>)] ([parameter,] <vector expression>)
or

<aggr-op>([parameter,] <vector expression>) [without|by (<label list>)]
```

# Metrics Type

[Metric types | Prometheus](https://prometheus.io/docs/concepts/metric_types/)

## **Counter**

A *counter* is a cumulative metric that represents a single [monotonically increasing counter](https://en.wikipedia.org/wiki/Monotonic_function) whose value can only increase or be reset to zero on restart. For example, you can use a counter to represent the number of requests served, tasks completed, or errors.

<aside>
🚨 Do not use a counter to expose a value that can decrease. For example, do not use a counter for the number of currently running processes; instead use a gauge.

</aside>

## **Gauge**

A *gauge* is a metric that represents a single numerical value that can arbitrarily go up and down.

Gauges are typically used for measured values like temperatures or current memory usage, but also "counts" that can go up and down, like the number of concurrent requests.

## Histogram

Histograms sample observations by their frequency or count, placing the observed values in pre-defined buckets. 

[an-introduction-to-the-four-primary-types-of-prometheus-metrics](https://chronosphere.io/learn/an-introduction-to-the-four-primary-types-of-prometheus-metrics/#:~:text=There%20are%20four%20primary%20Prometheus,gauges%2C%20histograms%2C%20and%20summaries).

[The histogram groups the data,across its range of values](https://andykuszyk.github.io/2020-07-24-prometheus-histograms.html#:~:text=The%20histogram%20groups%20the%20data,across%20its%20range%20of%20values).

# Useful Queries

API: Failure rate 4xx, 5xx(%) last 7 days

```sql
sum by (handler) (avg_over_time(http_requests_total{status=~"5..|4..", container="elastic-search-staging"}[7d])) / sum by (handler) (avg_over_time(http_requests_total{container="elastic-search-staging"}[7d])) * 100
```

API: Total request(s) by status code

```sql
sum by (status) (rate(http_requests_total{handler!="/"}[1m]))
```