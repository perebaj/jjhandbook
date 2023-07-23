# Complexity

I spent some time reflecting on what I think about Kubernetes, things like when and how it should be used, whether it is too complex system to solve any kind of problem, and the fact that for every problem there are tools that will fit better or worse, the choice and deployment of which is up to each engineer.

But my reflection was more biased towards understanding complex systems, and they don't have to be just about computing. Let's think, for example, about the Constitution of a nation. As tormenting as it may be, playing the game of life without knowing its rules that are described in the Constitution, it's a little weird for me, but everyone lives normally, we know one concept here and there, but that does not prevent us from living in society. Even so, it would be ignorant of me, as a solution to this, to say that the constitution should be simpler. The truth that I have realized for some time is that there are no simple systems for complex problems. You can follow patterns, adopt any measures you deem necessary for its construction, but it is also a fact that a work that tries to describe something like limits for a complex behavior will be complex as well.

I can observe very similar behaviors in software development, one of the college disciplines that amazed me the most was the complexity of operating systems, an example being all the abstraction for you to happily run an ls in your terminal, read a file, connect an external device to your machine, and so on. There are so many nuances and details, but still, all developers use Unix daily on their machines without knowing about them, but still build incredible things.

Finally, I think I can say something similar about Kubernetes. Does the developer behind their machine have the time and willingness to know about all the nuances of infrastructure, even if they are basic, to get a system up and running? Do they want to know how load distribution between different machines works? Details about network?Problems related to distributed systems in general? I can guarantee that there are some rare individuals who can brush up well on all these topics, but they are people with decades of experience in the field. And, in fact, these are important topics that consistent engineers should have knowledge of. But the moral is that most of the time you don't want to worry about this, you just want an abstraction to run your service in peace. Kubernetes is software that targets this pain. "I don't care what you give me to run, but I promise you that it will work, and all the knowledge behind it will be abstracted behind an API".