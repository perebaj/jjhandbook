Summarized content from the below link

[Channel go 101](https://go101.org/article/channel.html)

# Channels

This article will list all the channel related concepts, syntax and rules.

Channels make goroutines share memory by communicating. We can view a channel as an internal FIFO (first in, first out) queue within a program.

> ⚠️ Go channels can help programmers write data races free code easily, but Go channels can't prevent programmers from writing bad concurrent code from the syntax level. ⚠️

## Channels types and Values

Like array, slice, and map, each channel type has an element type. A channel can only transfer values of the element type of the channel.

Channel types can be **bi-directional** or **single-directional**. Assume `T` is an arbitraty type.

- `chan T` denotes a bidirectional channel type. Compilers allow both receiving values from and sending values to bidirectional channels.

- `chan <- T` denotes a send-only channel type;

- `<-chan T` denotes a receive-only channel type.

### Capacity/Buffer

Each channel value has a capacity, which will be explained in the section after next. A channel value with a zero capacity is called unbuffered channel and a channel value with a non-zero capacity is called buffered channel.

The below example will create a channel whose element type is `int`, the second argument specifies the capacity of the new channel.

```Go
make(chan int, 10)
```

## Channel Value Comparisons

TODO(read this article - <https://go101.org/article/value-part.html>)
If one channel value is assigned to another, the two channels share the same underlying part(s). In other words, those two channels represent the same internal channel object. The result of comparing them is true.

## Channel Operations

There are five channel-specified operations.

1. Close the channel
`close(ch)`

2. Send a value
`ch <- v`

3. Receive a value
`<-ch`

4. Query the value buffer capacity by using the following function call

`cap(ch)`

5. Query the current number of values in the value buffer

`len(ch)`

## Detailed Explanations for Channel Operations

To make the explanations for channel operations simple and clear, in the remaining of this article, channels will be classified into three categories:

- nil channels.
- non-nil but closed channels.
- not-closed non-nil channels.

| operation          | A Nil Channel  | A Closed Channel | A Not-Closed Non-Nil Channel |   |
|--------------------|----------------|------------------|------------------------------|---|
| Closed             | panic          | panic            | succeed to close             |   |
| Send Value to      | block for ever | panic            | block or succeed to send     |   |
| Receive Value From | block for ever | never block      | block or succeed to receive  |   |

# Resources

- [Channels use cases](https://go101.org/article/channel-use-cases.html)
