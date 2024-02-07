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
