# Ownership

It's normal that languages use some approach to manage memory. Or using a garbage collector, explict memomy allocaiton or deallocation. Rust uses a different approach, it uses the concept of ownership.

The code has a set of rules that the compiler checks at the compile time. If any of these rules are violated, the code will not compile. In that way, the code is safe and efficient.

## Difference between Stack and Heap in memory management

Most of the languages don't require the programmer to think about the memory management. But in Rust, you need to take care and have a deep understanding of the difference between the stack and the heap.

Accordingly the documentation.  The Stack stores values in the order it gets them and removes the values in the opposite order. This is known as LIFO (Last In, First Out). **All data stored on the stack must have a known, fixed size.** This approach is used to store variables, function calls, and references.

```
Data with an unknown size at compile time or a size that might change must be stored on the heap. This because the heap is less organized: when you put data on the heap, you request a certain amount of space. The operating system finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location. This process is called allocating on the heap and is sometimes abbreviated as just allocating.
```

A good example of this is when you want to find a spot for a group of friends in a restaurant. When you enter, the host checks if there is a table available. If there is, you are seated. If not, you wait until a table is available. When you leave the restaurant, you tell the host that you are done with the table, and it can be used by someone else. If some new friends arrive, the job of the host is to find a new chair for them.

This is because the stack is faster than the heap. Less overhead logic is needed to manage the stack!

In the heap, on the other hand, the memory is less organized. When you put data on the heap, you request a certain amount of space. The operating system finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location. This process is called allocating on the heap and is sometimes abbreviated as just allocating.

### ownership rules

- Each value in Rust has a variable that's called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.


## The String type


# References

- https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html
