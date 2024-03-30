# MongoDB

# Resources

- [How to start with Golang + MongoDB and hands on example](https://github.com/mongodb-university/atlas_starter_go/tree/master)
- [Newsletter - A personal project](https://github.com/perebaj/newsletter)


# Normal issues that developers commit using MongoDB

- Mongo offers a different paradigm, NOSQL, so, if you try to model your collections as tables and keep the same mindset, for example, using JOINs, you will face a tough environment. Therefore, it's a good idea to embrace the NOSQL paradigm to achieve good results.

- Secondly, use the data types correctly. This is a good practice for all databases, including graphs, columnar and so on.

- Thirdly, replication and sharding are very important for databases that use documents. So, to improve the performance and reliability of your system, consider this simple setup.

- Fourthly, use the right indexes to improve the performance of your queries, this is also a good practice for all databases, and it depends on the type of query, data and goals of your system.

- Fifthly, use and abuse the aggregation pipelines and give the database the duty to do the heavy work, not your application. So this idea to do 2 different queries and join them in your application using a "fake" foreign key, usually is a bad idea.
