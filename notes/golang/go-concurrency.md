# Basic concurrency

**Concurrency is a program's ability to do multiple things at the same time.**

Go doesn't provide a threading model for concurrency. Instead, it uses goroutines and channels. Concurrency is cheap and much easier to manage than traditional thread pools.

Languages such as Java and Python implement concurrency by using threads. Go takes a different route. Following a model proposed by the computer scientist **Tony Hoare**, using the model called **Communication Sequential Processes(CSP)**.

Two crucial concepts make Go’s concurrency model work:

- **Goroutines:** A goroutine is a function that runs independently of the function that started it. Sometimes Go developers explain a goroutine as a function that runs as if it were on its own thread.
- **Channels:** A channel is a pipeline for sending and receiving data. Think of it as a socket that runs inside your program. Channels provide a way for one goroutine to send structured data to another.

⚠️ ```
Goroutines and channels are one of the few places in the Go language where programmers can introduce memory leaks.``` ⚠️

In Go, functions are first-class. They can be created inline, passed into other functions, and assigned as values to a variable. You can even declare an anonymous function and call it as a goroutine, all in a compact syntax, as the following listing shows.

```go
package main
import (
    "fmt"
    "runtime"
)
func main(){
    fmt.Println("Outside a gorotine")
    go func (){ // <- Go routine inline
        fmt.Println("Printlb a goroutine")
    }()
    fmt.Println("Outside a goroutine")
}
```

## Technique 11 Waiting for goroutines

Sometimes you’ll want **to start multiple goroutines but not continue working until those goroutines have completed their objective.** Go wait groups are a simple way to achieve this.

> ⚠️ Wait groups (sync.WaitGroup), Wait until all of the workers are done! ⚠️

## Goroutines

A goroutine is a function that is capable of running concurrently with other functions

# Concurrency Issues

- Race Conditions: Multiple pieces of code, trying to access shared data

- Dead Locks: When a process waits for another one forever. This often happens when threads are competing for limited resources

- Live Locks: Similar to deadlock, but instead of being completely blocked, they continue executing actions without process

- Starvation: Occurs when a thread or process is prevented from making progress due the inadequate resource prioritization

# The thread pool concurrency model

The thread pool is a concurrency model that allocates or deallocates threads to execute programs in parallel

Creating and destroying a thread and its associated resources can be an expensive process in terms of time.

The number of threads may be dynamically adjusted during the lifetime of an application based on the number of waiting tasks. For example, a web server can add threads if numerous web page requests come in and can remove threads when those requests taper down. **The cost of having a lager thread pool is increased resource usage**.

The algorithm used to determine when to create or destroy threads affects the overall performance:

- Creating too many threads wastes resources and costs time creating unused threads;
- Destroying too many threads requires more time later when creating them again;
- Creating threads too slowly might result in poor client performance (long wait times).
Destroying threads too slowly may starve other processes of resources.

Resource: [Thread Pool](https://en.wikipedia.org/wiki/Thread_pool)

# CSP - Communication Sequential Processes

Channels is a one-way communications pipe

- Things go in one end, come out the other
- in the same order, they went in
- until the channel is closed
- multiple readers/writes

Concurrency is always hard (The human brain didn't evolve for this. +_+), and CSP provides a model for thinking about it that makes sit **less** harder**
Go doesn't force developers to embrace the asynchronous ways of event-driven programming.

The go routine could be related to threads, but they don't are. When you create new go routines, **behind the scenes the go is responsible to allocate the right number of threads depending on the hardware resources that you are using*

At the begging of the video, we conclude that if multiple pieces of code could access the same data, is very bad and hard to deal with, the way that Go deal with that is by transferring the ownership of data so that only one goroutine at a time is writing the data (avoid race conditions)

> Don't communicate by sharing memory, instead, share memory by communicationg" - Rob Pike

# Important Resources

[Concurrency in GO](https://livebook.manning.com/book/go-in-practice/chapter-3/9)
[Go class 23 CSP, Goroutines, and Channels](https://www.youtube.com/watch?v=zJd7Dvg3XCk&ab_channel=MattK%C3%98DVB)

# Go Tour - Concurrency

[Go Tour - Concurrency](https://go.dev/tour/concurrency/3)

## Channels

Channels are a typed conduit through which you can **send** and **receive** values with the channel operator, `<-`

⚠️ By default, sends and receives **block** until the other side is ready ⚠️. This allows goroutines to synchronize without **explicit locks or condition variables**

## Directional Channels

Channels can be direction - which means that you can restrict a channel to only send or receive data. This is specified by the arrow (`<-) accompanied by the channel declaration

For example

```go
out chan <- int
```

the `chan <-` declaration tells us that you can only send data into the channel, but not receive data from it.

## Buffered Channels

Channels can be buffered. Provide the buffer length as the second argument to `make` to initialize a buffered ch:

```golang
ch := make(chan int, 100)
```

Sends to a buffered channels block only when the buffer is full

## Range and Close

A sender can `close` a channel to indicate that no more values will be sent.

⚠️ Note: Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic. ⚠️

⚠️ Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a range loop. ⚠️

## Sync.Mutex

channels are good for communication, but what do we need to do when we want to make sure only one goroutine can access a variable at a time to avoid conflicts?

This concept is called **mutual exclusion**, and the conventional name for the data structure that provides it is a ``mutex``

