# [Interfaces - Russ Cox](https://research.swtch.com/interfaces)

Go's interfaces let you use [**duck typing**](https://en.wikipedia.org/wiki/Duck_typing).

    if it walks like a duck and it quacks like a duck, then it must be a duck

**With duck typing and object is of a given type if is has all emthods and properties required by that type**

Errors of wrong signatures and types in the **same moment that you are coding** not only in the **compile time**

Obs: There is a lot of low level information(About how it's possible to know at runtime if some object implements or not the passed interface), but just the beginning of the article it's enough to get a big picture of: What is an Interface?



[Go Interfaces - Ian Lance Taylos](https://www.airs.com/blog/archives/277)

A really nice text ^^

    Interfaces in Go are similar to ideas in several other programming languages: pure abstract virtual base classes in C++; typeclasses in Haskell; duck typing in Python; etc. That said, I’m not aware of any other language which combines interface values, static type checking, dynamic runtime conversion, and no requirement for explicitly declaring that a type satisfies an interface. The result in Go is powerful, flexible, efficient, and easy to write.
