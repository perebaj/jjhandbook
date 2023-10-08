- [Funkero-Cometa-Podcast](https://www.youtube.com/watch?v=oRMPoFsAtQQ&t=2116s&ab_channel=CometaPodcast)

About racism, rap, experiences of a guy from "Rio de Janeiro Favela", eropean and north america imperialism

---

- [John Carmack x Lex Fridman](https://www.youtube.com/watch?v=I845O57ZSy4&t=1s&ab_channel=LexFridman)

What Jhon think about topics like:
- Programming languages
- Hard work

---

- [Dave-Chaney Absolute Unit test](https://dave.cheney.net/2019/04/03/absolute-unit-test)

> - You are not allowed to write any production code unless it is to make a failling unit test pass;
>- You are not allowed to write any more of a unit test than is sufficient to fail, and compilation failures are failures;
>- You are not allowed to write any more production code than is sufficient to pass the one failling unit test
>
> Uncle Bob

> Dave Cheney
>
> - If i knew what I wanted to write, the writing the failing test first was easy
> - If i didn't know what I was writing, the cost of experimentation started at 200%; change the code, fix the test.
>
> - This cost went up and to the right when one function called another

In go the unit of code is the package.
Code coverage is your guide.
When you refactor, use coverage to tell you where you need to add tests.

---

# [Kubernetes: The Documentary](https://www.youtube.com/watch?v=BE77h7dmoQU&t=1s&ab_channel=Honeypot)

A Kubernetes history told by the legends of software

---

# [Is KUBERNETES Overly Complex?](https://www.youtube.com/watch?v=Ty5Tj4Jag_A&ab_channel=ContinuousDelivery)

---

# [REST API and OpenAPI: It’s Not an Either/Or Question](https://www.youtube.com/watch?v=pRS9LRBgjYg&ab_channel=IBMTechnology)

OpenAPI: Describe a REST API Interface
File, YAML or JSON that describe what the API/Service can do
Benefits:

- Standerdize Format: Readable for humans and machines
  - Describe API Resources
  - Parameters
  - Authentication
  - Operations
- Understand how to use
- Extend w/ tooling
  - API Doc
  - SDK

---

# [Introduction to NoSQL - Martin Fowler - GOTO 2012](https://www.youtube.com/watch?v=qI_g07C_Q5I)

- Impedance mismatch
  - Match different objects can cause difficulties

- (Middle of 1990 Ruse if ivhect databases). Take an object from memory and save it direct in disk without "fancy" mapping. (Principally when you are trying to integrate different systems).
- In the start of computing, system was designed to run in big boxes, as result the tools was building using this paradigm, the distruibuited systems wasn't so developed. The SQL was like that. builded to use bigbox, but in the end of 90 decade, companies like Google start to scale your servers using a lot of big boxes connected by network. That leaves to NOSQL movement. Google with Bigtable and Amazon with Dynamo
- It's hard to defined NoSQL databases because they born by "acident", them what we can do is find some shared caracteristics.
  - Non Relational
  - Open source
  - Cluster friendly
  - 21st Century Web
  - Schema-less
- In non relational databases you assume the fields names and content. Things Like: The field where the price lives is called price and not consumer-price, and so on. (Implicit schema).

Note: Imagine find data in a table that are spread across different clusters. This make sense? Like, half table are in one computer and other one in different computer?

- RDBMS == ACID
  - Atomicity
  - Consistency
  - Isolatated
  - Durability
- NoSQL == Base

- An example that I'm alredy live, are the case when same user are trying to update the same data into a document database. And obvius that a warning will be pop it up.
How can you circumvent that? Wrapping our request to try again? But how to do that when you have a user waiting a response and you have a limit time to do that.

When and why to use a SQL database?

easier development and larga scale data

An example, imagine an article system, you share the same content to all users, and when you have to update it, for my it's more simple imagine usa a documenta database to do that!

---

# [1º Meetup API Floripa - 10/05/2023](https://www.youtube.com/watch?v=moUVMdUzz8g&ab_channel=APIFloripa)

- Observabilidade de APIs usando golang. Some examples using middlewares to implement basic observability into your applications

MELT: Metrics/Events/Logs/Trace

- Transactional Outbox
  - Problem: When you try to commit a message to an application, but some shit occour in the middle.
    The proposed solution is to create an outbox database, that replicates all entities and other applications will consume the "messages" from this relational database and update the status when the message was actually delivered and consumed
About distributed systems. The question is when the problem will occur. You could mitigate them or trust in fairy tales that solve magically


---

# Designing Quality APIs (Cloud Next 18)

Briefing:
The main idea of this talk was that software is hard to change because it is complicated and permeated with assumptions, principally when you have integrations and versioning. Pay attention to the basics, in the beginning, to avoid messy software in the future

What is an API: Two pieces of software trying to communicate between network

The communication needs to be fast, secure, and easy to program

Software is hard to change:

- Because it is complicated
- Because it is permeated with assumptions

Some APIs styles offer an "attack" on important problems.

## 2 dominant styles of API

- Entity-oriented: my code manipulates the entities you expose

- Procedure-oriented: my code calls your procedure/function/method

## API properties to attack important problems

- Uniform API across all applications
- Uniform model for relationships
- Free of implementation assumptions

Unfortunately, HTTP tells you how to do CRUD, but not how to do query or versioning

<https://www.youtube.com/watch?v=P0a7PwRNLVU&ab_channel=GoogleCloudTech>

--

# Distributed System Course

[Distributed Systems Course](https://www.youtube.com/watch?v=cQP8WApzIQQ&list=PLrw6a1wE39_tb2fErI4-WkMbsvGQk9_UB&ab_channel=MIT6.824%3ADistributedSystems)

Ideas behind to use multiple computers connected by a network:

- parallelism
- fault tolerance
- physical

Abstractions implementations that hide distributed system complexity:

- RPC
- Threads
- Concurrency

Performance: Scalability - 2x computers -> 2x throughput -> 100x Complexity(kk)


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

# [Go Concurrency Patterns](https://www.youtube.com/watch?v=f6kdp27TYZs)

**History**

> Go cuncurrency model isn't new, it comes from Hoare's CSP in 1978 and even Dijkstra(1975)

**Gourotines**

- It's not a thread
- It has its own call stack, which grows and shrinks(decrease) as required
- It's very cheap. It's practical to ahve thousands, even hundreds of thousands of it
- There might be only one thread in a program with thousands of goroutines

**Buffered channels**

- Buffering removes synchrinization

**Paterns**



- 08:33 What is a goroutine ?
- 10:17 Channel
- 13:13 Buffered Channel
- 13:50 Go approach to concurrency
- 14:27 Generator Pattern (return Channel)
- 16:58 Multiplexing 2 Channels
- 20:13 Select statement
- 22:47 Time out using Select
- 24:26 Quit using Select
- 26:28 Daisy chain
- 29:26 Google search example
- 37:34 Don't over do ; keep it simple
- 39:08 Conclusion

# [Why Generics](https://go.dev/blog/why-generics)

Statically typed langue:

In this type of language the data type of a variable is known at the compile time which means the programmer has to specify the data type of a variable **at the time of its declarations**

    Generic programming enables the representation of functions adn data structures in a generic form, with types factored out.

Reverse a slice of int

```Golang
  func ReverseInts(s []int) {
    first := 0
    last := len(s) - 1
    for first < last {
      s[first], s[last] = s[last], s[first]
      first ++
      last --
    }
  }
```

    Go is a statically typed language because that maskes it easier to write large programs, we don't want to lose the benefits of static typing in order to gain the benefits of generics

    Ian Taylor


## Benefits and costs
Gerenics can open in the language a new variety of data structure

    Every language change has a cost. There’s no doubt that adding generics to Go will make the language more complicated.
    
    As with any change to the language, we need to talk about maximizing the benefit and minimizing the cost.

    We reduce complexity by making the individual features simple, and we maximize the benefit of the features by permitting their free combination. 


[Getting started with generics](https://go.dev/doc/tutorial/generics) 
