# A philosophy of Software Design


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
