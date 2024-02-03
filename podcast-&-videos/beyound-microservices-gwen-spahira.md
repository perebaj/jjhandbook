# [Beyond Microservices: Streams, State and Scalability • Gwen Shapira • GOTO 2020](https://www.youtube.com/watch?v=H0LUi51aCP8)

## Microservices

- **Clients know too much**
    What service should be open to the clients?

- **Shifts in responsibility, redundancy**

    Try to answer the question: "What service is responsible for this data?" & "What is going on in the system? Where is the error?"

- **Making changes is Risky**

    Also having good contracts and APIs, changes are risky. This idea that it's easy to change microservices because they are decoupled, is not 100% true.

- **Adding services requires EXPLICIT coordination and call**

    You need to convince the satellite clients that your service has value, and will save their time & money without creating a lot of new problems.
- **Rest = HTTP + JSON = Slow**


## We can do better

As the number of services grows, you need to ensure the connection between services is fast, smooth, and resilient.

- **API Gateway**

    Response to: Clients know too much. It's a single point of entry for all clients. It's a good place to do **authentication**, **authorization**, and **rate limiting**.

    Responsibilities:
        - Authentication
        - Routing
        - Rate limiting
        - Logging and analytics

- **Service Mesh**

    The service mesh is an answer to the problem that the communication between services across the network includes.

    It coordinates the communication between services, taking care of load balancing, retries, timeouts, and circuit breaking.

    To do that, it injects a sidecar proxy into each service instance, and the sidecar proxy is responsible for the communication between services.

    Obs: Sidecar is a container that has these functionalities, and is deployed together with the service.

# Conclusion

The content captures the ideas and problems that microservices add when the number of services grows. The idea of API gateway and service mesh was good for understanding that engineers are always creating new tools to solve problems when they become bigger and bigger.
