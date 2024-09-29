# NoSql x Sql

Pretty basic topic, but let's write it down.

SQL is the type of database that fit for the most of the cases. It enable you to do complex queries and the principal aspect, create relationships between tables.

But do to it in the right way, you need to have a good understanding of your data and how it is related. This is the main point of SQL, the relationships.

Other aspect, is that SQL databases offers the ACID properties, which is a good thing for most of the cases.

A = Atomicity = It means that each type of transaction is tread as a single unit. Weither all the operations are completed or none of them are.

C = Consistency = The DB should start and finish in a valid state.

I = Isolation = Each transaction should be isolated from the others.

D = Durability = Once a transaction is completed, it should be permanent.

For this rules, it's a little bit tricky to scale SQL databases. The most common way is to increase the size of the machine, but is has a limit. For this reason, you need to think a little bit more about the architecture of your system and data.

On the other hand, NoSQL databases are designed to scale horizontally. It means that you can add more machines to your cluster and the database will be able to handle it (sharding).

But it's not so simple as it seems. NoSQL aren't easy for complex queries and if you try to create relationships between tables, you will face a tough environment.

But it has some advantages, like the schema-less, which means that you can prototype your application faster.

Other aspect is that NoSQL has different types of databases, like document, key-value, graph and so on. This means a better fit for your problem.

Example of applications that I already worked with NoSQL databases:

- **Document**: MongoDB = Save data of temperatur, presure and humidity of a sensor. Temporal series. Logs of a system.

- **Key-Value**: Redis = Cache of a system. Save the last 10 queries of a user.

---

SQL example: Transactions of a back or the intention of purchase of an user.
