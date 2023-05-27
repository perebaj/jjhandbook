Some basic thoughts about distributed programs

When I had my first contact with parallelism and concurrency, I always suspected that was something too advanced that I probably would use in sophisticated projects. I was wrong.

Although the backend applications I built always had to deal with databases and some third-party API, I never had this strong conviction that starting your system in a concurrent disposition was indispensable.

Let's think for example about the cost to run and deploy something in a cloud environment. Taking away the developer cost. To minimize the amount of money you invest in your software-focused problem, you want your machines to be overused. Achieving that or something close is evidence of well-thought-out engineering. Thinking about your application in a way that concurrency and asynchrony are done well is a crucial checkpoint to achieving an advanced level of engineering.

Today I think like that, but in the past, I don't think so. I strongly believe that starting to use abstractions like serverless was a factor that masked this necessity, which I now see as fundamental.

What I want to emphasize here is that knowing where your software will run and the tools that you will use(such as the programming language), is fundamental to the design process of your service, testing, and even the way that you monitor the application and display logs. The problem becomes much more complex when you are trying to integrate different services with the same behavior. This is where the true evil of distributed systems lies.


