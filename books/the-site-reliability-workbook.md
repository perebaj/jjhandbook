# [The site reliability workbook](https://sre.google/workbook/table-of-contents/)

## [Chapter 6: Monitoring distributed systems](https://sre.google/sre-book/monitoring-distributed-systems/)

- Why Monitoring
- settings reasonable SLOs(Service level objectives)
    - I don`t like to use magic monitoring tools, that will analyze the metrics and tell you what is wrong. - Make it simple!🫡
        - Monitoring your queues, your databases, your API requests, CPU, and memory usage or even if the deployment was successful.
- Symptoms vs causes
    - The **what is broken indicates the symptom**, the **why** the possible cause.
    Examples:

    - **Symptom**: The site is slow
    - **Cause**: The database is overloaded

    - **Symptom**: We are receiving 500 errors from our API
    - **Cause**: The API is down/overloaded
- Blackbox vs whitebox monitoring:

    **Blackbox:** it's like a macro view of your system, you don`t know what is happening with details, but you know that something is wrong.

    Isn't too useful for eminent problems that have not happened yet.

    **Whitebox:** Detailed logs and instrumentation of specific components.

## The Four golden signals

- **Latency**: The time it takes to service a request.

- **Traffic**: The amount of requests your system is receiving.

- **Errors**: The number of failed requests.

- **Saturation**: How "full" your system is
