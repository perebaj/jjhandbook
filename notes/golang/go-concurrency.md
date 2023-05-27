# Basic concurrency


**Concurrency is a program's ability to do multiple things at the same time.**

Go doesn't provide a threading model for concurrency. Instead, it uses goroutines and channels. Concurrency is cheap and much easier to manage than traditional thread pools.

Languages such as Java and Python implement concurrency by using threads. Go takes a different route. Following a model proposed by the computer scientist **Tony Hoare**, using the model called **Communication Sequential Processes(CSP)**.

Two crucial concepts make Go’s concurrency model work:

- **Goroutines:** A goroutine is a function that runs independently of the function that started it. Sometimes Go developers explain a goroutine as a function that runs as if it were on its own thread.
- **Channels:** A channel is a pipeline for sending and receiving data. Think of it as a socket that runs inside your program. Channels provide a way for one goroutine to send structured data to another.

⚠️ ```
Goroutines and channels are one of the few places in the Go language where programmers can introduce memory leaks.``` ⚠️


## Goroutines 

A goroutine is a function that is capable of running concurrently with other functions



# Important Resources

[Concurrency in GO](https://livebook.manning.com/book/go-in-practice/chapter-3/9)