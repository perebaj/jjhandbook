# [Why Go ? - Russ Cox](https://www.youtube.com/watch?v=XvZOdpd_9tc)

- Go is designed to solve concurrency problems that pop it on could environment
- Just enough to you build your own custom solutions

> Do less enable more
>
> Keeping the language small enables more important goals. Being small makes Go easier to learn, easier to understand, easier to implement, easier to reimplement, easier to debug, easier to adjust, and easier to evolve. Doing less enables more.
>

How should we structure and coordinate concurrent and parallel computations?

Mutexes and condition variables are very general but so low-level that they’re difficult to use correctly. Parallel execution frameworks like OpenMP are so high-level that they can only be used to solve a narrow range of problems. Channels and goroutines sit between these two extremes.

types and interfaces. Having static types enables useful compile-time checking, something lacking in dynamically-typed

**dynamic typed languages** = Python or Ruby

**Building and sharing**


Building and sharing software. In the run up to Go 1, we built goinstall, which became what we all know as “go get”. That tool defined a standard zero-configuration way to resolve import paths on sites like github.com, and later a way to resolve paths on other sites by making HTTP requests.

Open Source

Open the development process. The process would be the same if you work for Google or not

Communicate your vision for the laguange to enable that other programmers ship new things using Go
