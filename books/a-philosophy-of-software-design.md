# A philosophy of Software Design - John Ousterhout

In this book, John Outerhout specifically talks about software decomposition, when is the best time to split code, make it more modular, and what techniques and principles can be taken into account to make these decisions!

A question before continuing: What differentiates a good developer from a great developer?

According to the book "Talent Is Overrated: What Really Separates World-Class Performers from Everybody Else", talent is a mix of a few factors:

* *Prática deliberada*
* *Dedicação*
* *Resiliência*

In short. Working smart, focused, and with clear goals is what makes anyone, not just developers, better at what they do.

## Chapter 1 - Introduction (It's all about complexity)

* The bigger the program and the more people working on it, the harder it is to manage complexity

* Complexity will still increase over time. Despite our best efforts, simpler designs allow us to build larger, more powerful systems.

* There are 2 general approaches to dealing with complexity:

* Eliminate it by making it simpler and more obvious.

* Encapsulate it.

* Encapsulation is the process of hiding complexity behind a simpler interface. It allows us to build complex systems without having to understand all the details of how they work.

Unlike physical projects (bridges and buildings) and specific projects, where you have to visualize exactly what you want to build before you even start, software has a different characteristic.

What was used in the past was a code production model called the `waterfall model`, which aimed to design the system completely before it was even implemented. As a result, the initial design had many problems, problems that only appeared when implementation began.

Because of these problems, most projects began to adopt agile development, based on incremental software development, where it is emphasized that the first deliveries are just a small piece of a final delivery, which is gradually increased as the product is evaluated by customers.

This practice also reflects the technical level of your project, since in each iteration, new problems arise and are corrected before new features are even added.

In other words, incremental development means that the software is never ready; the design happens constantly throughout the life of the project.

## Chapter 7 - Different layers, different abstractions

If the systems constains layers with similar abstractions, this is a red flag that suggests a problems with class decomposition. But in some cases this is not a problem, for example:

**Dispatcher**

A component resposible to directing or transfering control, data or tasks to the appropriate component or layer. It select one of several possible methods and to do that, it need to have the same signature of the methods it is dispatching to.

**Decorators**

Red flags:

- A pass-through method is one that does nothing except pass its arguments to another method.

## Chapter 8 - Pull Complexity Downwards

- usually you have more users than developers. So move the complexity to the users couldn't be a good idea.
- Move the complexity to the code/class/method, could be noise, but this can save user time/overhead.
- Sometimes we manage complexity through parameters and configuration, but this can be a problem. How the user will know if the 1000 limit size for their LRU cache is better than 1001? Sometimes this freedom is not too good.

# Chapter 9 - Better Together or Better Apart

Given two pieces of functionality, should they be implemented together in the same place, or should their implemented be sepated?

Should the parsing of an HTTP request be implemented entirely in one method, or should it be divide among multiple methods(or even multiple classses)?

It might appear that the best way to achieve this goal is to divide the system into a large number of small components: the smaller the components, the simpler each individual component is likely to be. However, the act of subdividing creates additional complexity that was not present **before** the division.

* The more components, the hards to keep track of them all and the harder to find a desired component within the large collection.
* Subdivision can result in addiotional code to manage the components.
* Subdivision creates separation: the subdivided components will be farther apart than they were before subdivision. It makes harder for devs to see the components at the same time, or even to be aware of their existence.
* Subdivision leads duplication.


Bring pieces of code together is most benefical if they are closely related. Here are a few indicators that two pieces of functionality are closely related:

* Thet share information
* They are used together. Anyone using one of the pieces of code is likely to use the other as well.
* They overlap conceptually. They are part of the same abstraction, or they are part of the same conceptual model.
* Its hard to understand one without understanding the other.
