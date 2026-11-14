NO SQL

My experience with databases, most of the time, has included a lot of frustration. It's an art of computing that really needs specialists to model and understand a problem so that it fits like a glove inside the right database and doesn't add complexity or slow down the development speed of your project.

Since I've been extremely anxious for a while now about not having strong opinions on things I consider elementary in software development, I tried to make some basic reflections on how the choice between a relational database and a NoSQL one should be made according to the problem you have, and again, in the spirit of making myself start to think and form fundamental concepts, already assuming that this matters more than tooling.

Let's think about the clear difference between the two types of database. What could have led someone to think that the whole process of receiving an object in memory, instead of modeling it so that search and query operations are optimized, could become something like simply taking the object in memory and saving it directly to your hard drive?

Some things I found out were that SQL databases, or RDBMS, were built by design to be used on giant machines, which actually makes a lot of sense. At least so far, I've never heard of a relational database that has a table distributed across different clusters. Unlike object-oriented databases: with these, right off the bat, you can tell they are strongly tied to distributing data across many nodes of a mesh of computers.

The interesting point here is that this is fundamental information for understanding the usefulness of each tool. It's no coincidence that NoSQL databases gained traction in the market right when Big Tech started evolving strongly in distributed programming, at the end of the 90s. I believe the beginning of cheaper hardware must have had a huge influence on this.

These databases that came along, which I'd say sit at the top of a topology, group several other approaches to how data is saved, for example, document-oriented, key-value, graphs and so on...
But thinking about something they have in common, I think we can list 3 main aspects:
- Non Relational
- Cluster friendly
- Schema-less
