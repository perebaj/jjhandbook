# [Orchestrating Path of Success](https://www.infoq.com/podcasts/orchestrating-path-success/?utm_source=infoqEmail&utm_medium=editorial&utm_campaign=ArchitectNL&utm_content=01312025)

- Is that orchestration tool reliable? Is the same when you are using a cloud provider. Is that cloud provider secure? Probably it's way more secure than the things that you are building internally.

## Orchestration vs. Event-Driven Architectures

Orchestration X Choreography

- Orchestration: A central component that controls the flow of the system. It's a central point of control. It's a central point of failure. It's a central point of monitoring. It's a central point of logging
- Choreography: A decentralized approach. Each component knows what to do. It's a decentralized point of control. It's a decentralized point of failure. It's a decentralized point of monitoring. It's a decentralized point of logging


Bernand Ruecker:

> Because in my whatever, order fulfillment process, I need to talk to the CRM system, to the ERP system, to the stock, to the whatever, logistic system, so I need to get all those pieces together. So the question is how do those components interoperate? And with orchestration, basically I have one component that has the duty, the responsibility, of orchestrating the end-to-end process and saying, "Okay, the first thing I have to do is, for example, getting the stuff from stock or retrieving the payment", and so on and so forth. It can control that, to some extent. With event-driven, with the choreography, you basically distribute that responsibility, saying, "There's an event, the order just came in", and then some component knows, okay, then I have to collect money.

- The real problem is most systems are architectural spaghetti.
- The Architect is the Translator Between Business and IT


## References

[Why service collaboration needs choreography and orchestration](https://blog.bernd-ruecker.com/why-service-collaboration-needs-choreography-and-orchestration-239c4f9700fa)
