# [The site reliability Engineering](https://sre.google/sre-book/table-of-contents/)

## [Chapter 6: Monitoring distributed systems](https://sre.google/sre-book/monitoring-distributed-systems/)

## Why Monitoring?

It's impossible to measure the efficiency of something that isn't being monitored.

- Analyse long-term term trends
- Compare
- Alerting

The above three reasons are the main reasons to monitor your system.

Be able to compare the current state and have some visibility of what is going on, being less reactive and more proactive.

## Settings Reasonable SLOs(Service level objectives)

TODO

## Symptoms vs causes
    - The **what is broken indicates the symptom**, the **why** the possible cause.
    Examples:

    - **Symptom**: The site is slow
    - **Cause**: The database is overloaded

    - **Symptom**: We are receiving 500 errors from our API
    - **Cause**: The API is down/overloaded

# Blackbox vs whitebox monitoring:

**Blackbox:** it's like a macro view of your system, you don`t know what is happening with details, but you know that something is wrong.

Isn't too useful for eminent problems.

**Whitebox:** Detailed logs and instrumentation of specific components.

## The Four golden signals

Basic metrics that you should pay attention to:

**Latency**:

The time it takes to service a request.

**Traffic**:

The amount of requests your system is receiving.

- For an API, it could be the number of requests per second.
- For an Audio streaming service, the I/O bandwidth.
- For a database, the number of transactions per second.

**Errors**:

The number of failed requests.

**Saturation**:

How "full" your system is

## Worrying About Your Tail

**Mean is not a good metric for distributed systems**, because it can hide the real problems.

A good approach to deal with that is to use percentiles. Put metrics **related to time** our **consumption** into some buckets and analyze them.

## Resolution

**The granularity**

The granularity of the data you are collecting is also important, if you choose a very high granularity(in other words, a lot of data per second), you will overload your systems and increase the costs a lot.

## Conclusion

After gathering all these concepts, and putting them in production, some simple questions should be answered, like:

- Can I take action in response to this alert?
- Does this alert definitely indicate that the user is being affected?
