* [Instrumentation](#instrumentation---três-tipos-de-serviços)
    * [online-serving](#online-serving)
    * [offline-processing](#offline-processing)
    * [batch-jobs](#batch-jobs)
* [Agregação](#agregação)
    * [Sum x Count](#sum-x-count)
    * [Operadores de agregação e agrupamento](#operadores-de-agreção-e-agrupamento)
* [Metric Type](#metrics-type)
    * [Counter](#counter)
    * [Gauge](#gauge)
    * [Histogram](#histogram)
* [Useful Queries](#useful-queries)






# Instrumentation - Três tipos de serviços

[Instrumentation | Prometheus](https://prometheus.io/docs/practices/instrumentation/#instrumentation)

## Online Serving

Tipos de serviço que esperam uma resposta imediata. Por exemplo, uma requisição a um banco de dados.

As principais métricas em tal sistema são o número de consultas realizadas, erros e latência. O número de solicitações em andamento também pode ser útil.

## Offline-processing

Tipo de sistema que você não espera a ativamente sua resposta.
Para cada estágio é importante acompanhar os itens que chegam, quantos estão em progresso, a última vez que você processou algo ou até mesmo qual é a relação entre itens que saem/entram.

## Batch Jobs

Tem uma caracteristica peculiar que, não são tipos de sistemas que rodam frequentemente, tornando a coleta de suas métricas um pouco mais difícil.

Uma métrica chave para serviços em *batch* é a última vez que ele rodou com sucesso.

Qual o foi o tempo que cada etapa demorou

# Agregação

## Sum x Count


A principal diferença entre as duas funções é que "sum" retorna a soma dos valores da métrica, enquanto "count" retorna o número de amostras da métrica. Em outras palavras, "sum" é uma função de agregação que lida com valores numéricos, enquanto "count" é uma função de agregação que lida com a contagem de amostras.

--- 

## Operadores de agreção e agrupamento
Os operadores de agregação podem ser usados

Esses operadores podem ser usados para agregar as métricas do Prometheus em todas as dimensões de rótulos ou preservar as dimensões distintas, incluindo uma cláusula "without" ou "by". Essas cláusulas podem ser usadas antes ou depois da expressão.

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