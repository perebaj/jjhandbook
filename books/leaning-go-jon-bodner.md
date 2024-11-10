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

# Chapter 10 - Concurrency in Go

1) People are attrached to concurrency because they believe concurrent programs are faster. Unfortunately, that is not always the case. More concurrency doen't automatically make things faster.

```
“more concurrency does not mean more speed.”
```

2) “Another important thing to note is that concurrency isn’t worth using if the process that’s running concurrently doesn’t take a lot of time. This is why concurrent operations are often used for I/O”

## Goroutines

- Process

“A process is an instance of a program that’s being run by a computer’s operating system.”

- Threads

“A process is composed of one or more threads.”

### Select

The select algorithm is simple: it picks randomly from any of its cases that can go forward; order is unimportant. This is very different from a switch statement, which always chooses the first case that resolves to true. It also cleanly resolves the starvation problem, as no case is favored over another and all are checked at the same time.


```Go
“select {
case v := <-ch:
    fmt.Println(v)
case v := <-ch2:
    fmt.Println(v)
case ch3 <- x:
    fmt.Println("wrote", x)”
case <-ch4:
fmt.Println("got value on ch4, but ignored it")
}
```

Se na primeira iteração sobre o canal v, ele estiver "bloqueado", a própria engine do Golang
