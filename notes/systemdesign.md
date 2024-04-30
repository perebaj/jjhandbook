# System Design

Some core foundation knowledge about systems

**Design fundamentals**

- Defend out position
- Prove your point

Tools: Have a basic understanding of the **available instruments** that should be connected to build a system.

# Client Server model

A very basic approach to understanding it is trying to comprehend what happens when we try to access some URL in our browser.

Basically, every URL it's a map for an IP address, and our browser is responsible for making this whole process happen.

Each IP address has a lot of ports, that represent an apartment in a building. And you can expose in each of these ports a different program.

Usually, the ports that public services use are:

- HTTPS: port 443
- HTTP: port 80

Besides that, we have the client and the server. To components that establish connections to send and receive data.

Usually, the client is the browser, the user or someone else that is trying to access something, on the other hand, the server is the machine that has the data, and it's responsible for sending it back to the client.

# Network Protocols

Set of rules that establish patronized communication between two sides.

A good analogy could be the language, if two people don't speak the same language, they can't communicate. The same happens with serves and clients. The engineers create a set of patterns that makes possible communication between them.

## Protocols

- **IP**: Internet Protocol - All current internet uses this protocol, and it is formed by IP packages, which are bits formed by headers and data.

- Order and Size: The IP protocol by itself doesn't have a reliable way to send packages in an ordered way or even manage chunks of packages that are bigger than the maximum size of a package. For this reason, other protocols were created.

- **TCP**: Transmission Control Protocol - Introduced to solve the problems of the IP protocol, for example adding in the headers other information to make possible the ordered.

- **HTTP**: Hypertext Transfer Protocol - It's a protocol that uses TCP to send and receive data. It's the protocol that browsers use to communicate with servers.

# Load balancer x API Gateway

Summarizing, both are layers related to network and traffic control, but the load balancer acts in the redirection of the traffic to the right servers, avoiding overloading, otherwise, the API gateway is a layer between the client(user) and the services, in the sense that this tool helps in the management of authentication, authorization.
