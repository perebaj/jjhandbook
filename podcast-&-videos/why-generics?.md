# [Why Generics](https://go.dev/blog/why-generics)

Statically typed langue:

In this type of language, the data type of a variable is known at the compile time which means the programmer has to specify the data type of a variable **at the time of its declaration**

    Generic programming enables the representation of functions and data structures in a generic form, with types factored out.

Reverse a slice of int

```Golang
  func ReverseInts(s []int) {
    first := 0
    last := len(s) - 1
    for first < last {
      s[first], s[last] = s[last], s[first]
      first ++
      last --
    }
  }
```

    Go is a statically typed language because that makes it easier to write large programs, we don't want to lose the benefits of static typing to gain the benefits of generics

    Ian Taylor


## Benefits and costs
Generics can open in the language a new variety of data structure

    Every language change has a cost. There’s no doubt that adding generics to Go will make the language more complicated.

    As with any change to the language, we need to talk about maximizing the benefit and minimizing the cost.

    We reduce complexity by making the individual features simple, and we maximize the benefit of the features by permitting their free combination.


- [Getting started with generics](https://go.dev/doc/tutorial/generics)
