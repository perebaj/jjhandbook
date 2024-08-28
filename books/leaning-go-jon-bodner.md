# Learning Go - Jon Bodner

# Chapter 6: Pointers

- zero value equals nil: pointer, slice, maps and functions.
- the & is the address operator that returns the address of the memory location.
- * is the indirection operator. It returns the pointed-to-value. This is called dereferencing.
- begore dereferencing a pointer, you should check if it is nil. Otherwise you will get a panic.
- Modern software engineering embraces immutability.
```
[I]mmutable types are safer from bugs, easier to understand, and more ready for change. Mutability makes it harder to understand what your program is doing, and much harder to enforce contracts.
```

- Go uses pointers to declare that some variables and parameters are immutable.
    - Go developers use pointers to indicate that a parameter is mutable.
