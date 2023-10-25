[modules-monoliths-and-microservices](https://tailscale.com/blog/modules-monoliths-and-microservices/)

An especially powerful form of attack is getting the ability to commit code, because that code will eventually be run on developer machines, and some developer or production machine somewhere probably has access to do what you want to do.

---

Instead, module boundaries typically follow Conway's law. People break up modules based on how they want to subdivide the development work on their team, and modules end up communicating based on how the teams and teammates communicate. (Conway's law is fascinating and real, but you can read about it in many other places. Let's skip it for now.)

What module boundaries don't do is define the size of a unit of deployment.

```
Conway's Law is a principle in software development and organizational design that was formulated by computer programmer Melvin Conway in 1967. The law states:

"Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations."
```

Where should I put my service boundaries?

- Isolation
- Connection
- Compatibility guarantees
- Upgrades, downgrades, and scalability
