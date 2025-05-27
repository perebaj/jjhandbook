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
