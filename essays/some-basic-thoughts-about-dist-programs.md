Some basic thoughts about distributed programs

When I had my first contact with parallelism and concurrency, I always suspect that was something too advanced that I probably would use in sophisticated projects. I was wrong.

Although in the backend applications that I built, they always had to deal with databases and some third API, I'm never had this strong thought that starting your system in a concurrent disposition it's indispensable.

Let's think for example about the cost to run and deploy something in a cloud environment. Taking away the developer cost. To minimize the amount of money that you put into your problem(focused on the software), you want that your machines are overused, achieving that or something close, is evidence of well-think engineering. And again, thinking in your application in a way that concurrency and asynchronism are well done, I consider that it's a crucial checkpoint to achieving an advanced engineer level.

Today I think like that, but in the past, I don't think so. I strongly believe that starting to use abstractions like serverless was a point that masked this necessity that today I see as fundamental.

What I want to emphasize here is that should know where your software will run and the tools that you will use(like the programming language), is fundamental to the design process of your service, testing and even the way that you monitor the application and display logs. The problem becomes much more complex when you are trying to integrate different services with the same behavior. This is where the true evil of distributed systems lies, and few gems in the world of software have mastered this domain.




