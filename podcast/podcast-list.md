- [Funkero-Cometa-Podcast](https://www.youtube.com/watch?v=oRMPoFsAtQQ&t=2116s&ab_channel=CometaPodcast)

About racism, rap, experiences of a guy from "Rio de Janeiro Favela", eropean and north america imperialism

---

- [John Carmack x Lex Fridman](https://www.youtube.com/watch?v=I845O57ZSy4&t=1s&ab_channel=LexFridman)

What Jhon think about topics like: 
* Programming languages
* Hard work

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

- [Kubernetes: The Documentary](https://www.youtube.com/watch?v=BE77h7dmoQU&t=1s&ab_channel=Honeypot)

A Kubernetes history told by the legends of software


---

[Is KUBERNETES Overly Complex?](https://www.youtube.com/watch?v=Ty5Tj4Jag_A&ab_channel=ContinuousDelivery)


---

[REST API and OpenAPI: It’s Not an Either/Or Question](https://www.youtube.com/watch?v=pRS9LRBgjYg&ab_channel=IBMTechnology)

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


[Introduction to NoSQL - Martin Fowler - GOTO 2012](https://www.youtube.com/watch?v=qI_g07C_Q5I)

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