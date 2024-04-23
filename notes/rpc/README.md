# RPC

The RPC idea was born between the 70s and 80s with the wide necessity of communication between programs that are being run in distributed systems. The idea is to allow a program in a computer could invoke functions in a different program in another computer, like if we are calling local functions, making the communication between distributed systems more transparent.

# [gRPC Principles](https://grpc.io/blog/principles/)

## Motivation

Google already had a RPC architecture called Stubby to connect the large number of microservices running within and across Google data centers for over a decade. This internal system has gained popularity in the advent of other public releases, like SPDY, HTTP/2 and QUIC, for this reason, they decided to create this open-source project that already has nice features and with the goal to standardize the pattern that Google has being already using for a decade

**Promote the microservices design philosophy of coarse-grained message exchange between systems.**

## [Fallacies of distributed computing (Peter Deutsch)](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)

The fallacies are:
- The network is reliable;
- Latency is zero;
- Bandwidth is infinite;
- The network is secure;
- Topology doesn't change; - This is related to the arrangement of nodes and links and that this components doesn't remains constant, they can change due to hardware failures, upgrades or reconfigurations.
- There is one administrator;
- Transport cost is zero;
- The network is homogeneous. - Maybe this means that the network is consistent and uniform in its characteristics and components?

## [Microservices and the First Law of Distributed Objects](https://martinfowler.com/articles/distributed-objects-microservices.html)

In this article, Martin finds a contradiction in your past expositions about system design, because in some epochs he advised that distributing objects between different systems is a bad design, but this goes in contradiction to the microsystems design, that he has been exposing in the past few years. Here he tries to tease the real efficacy of these approaches;

Meanwhile, microservices are simpler, you need to take care of problems that are less explicit(generally related to network issues), and that are harder to deal with and figure out when things go wrong.

Even so, he says that he has a positive gut feeling for Monolithic architectures and that we have few examples in the software industry that have been running for wide years to be compared with Monolithic

**Distributed objects**

    The idea of distributed objects is that you could design objects and choose to use these same objects either in-process or remote.

Remote: Might mean another process in the same machine, or on a different machine.

In-Process: Is fast and always succeeds

    While small microservices are certainly simpler to reason about, I worry that this pushes complexity into the interconnections between services, where it's less explicit and thus harder to figure out when it goes wrong. Refactoring becomes much harder when you have to do it across remote boundaries. Microservice advocates tout the reduction of coupling you get from asynchronous communication, but asynchrony is yet another complexity booster.


# [Introduction do gRPC](https://grpc.io/docs/what-is-grpc/introduction/)

**Protocol Buffer:** Is a free and open-source data format used to serialize structured data.

    In gPRC, a client application can directly call a method on a server application on a different machine as if it were a local object, making it easier for you to create distributed applications and services.

# Protobuf

- In the message definition, we set the field with an integer, like that:

```protobuf
message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;
}
```

Because, in that way we can traffic the data by using the integer as a key, reducing the size of the message, and making it more efficient.

## Important Resouces

- (https://aprendagolang.com.br/2023/07/13/implementando-uma-api-com-protobuf-e-grpc/)
- (https://aprendagolang.com.br/2023/06/22/o-que-e-e-como-utilizar-protocol-buffers/)
