# [1º Meetup API Floripa - 10/05/2023](https://www.youtube.com/watch?v=moUVMdUzz8g&ab_channel=APIFloripa)

- Observability of APIs using Golang. Some examples of using middleware to implement basic observability into your applications

MELT: Metrics/Events/Logs/Trace

- Transactional Outbox
  - Problem: When you try to commit a message to an application, but some shit occurs in the middle.

The proposed solution is to create an outbox database, that replicates all entities and other applications that will consume the "messages" from this relational database and update the status when the message was delivered and consumed
About distributed systems. The question is when the problem will occur. You could mitigate them or trust in fairy tales that solve magically
