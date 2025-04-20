# Http transport layer

The http transport layer in Go is responsible for managing the underlying network connections and protocols used to send and receive HTTP requests and responses. It provides a way to customize the behavior of HTTP clients and servers, including connection pooling, timeouts, and proxy settings.

If you are creating a http client that calls a third-party API multiple times, you can use the http.Transport struct to manage the underlying connections. The http.Transport struct provides a way to customize the behavior of HTTP clients, including connection pooling, timeouts, and proxy settings.

Connection Pooling: Is a technique used to manage a pool of network connections that can be reused for multiple requests. This can help improve performance by reducing the overhead of establishing new connections for each request. The http.Transport struct manages a pool of connections for you, so you don't have to worry about it.

Timeouts: Are used to specify how long to wait for a response from the server before giving up. You can set timeouts for the entire request, or for specific parts of the request, such as the connection or the response body. The http.Transport struct allows you to set timeouts for the entire request, but you can also set timeouts for specific parts of the request using the context package.

Proxy Settings: Are used to specify a proxy server to use for outgoing requests. This can be useful for routing requests through a specific network or for bypassing firewalls.

# Concurrency Primitives

Normal concurrent language contructs their concurrency over the CPU threads. The basic calculation is that the number of threads is equal to the number of CPU cores times 2. So if you have 8 CPU cores, you can create 16 threads. This is the basic calculation for concurrency in most languages.
In Go, the concurrency model is based on a internal scheduler that manages the execution of the concurrent workers(goroutines). The scheduler is responsible for distributing the workload across the available CPU cores and memory. This allows Go to efficiently utilize the available resources and achieve high levels of concurrency without the need for explicit thread management.

# [Retry Go](github.com/avast/retry-go)

# Mutex

Mutex is a Golang primitive resource that enable a safe way to avoid race conditions. Imagine that you are saving some data in a file. We can face some conflicts in that operation. For example, if two goroutines are trying to write to the same file at the same time, one of them may overwrite the other's data. To avoid this, we can use a mutex to lock the file while one goroutine is writing to it. This way, only one goroutine can access the file at a time, preventing any conflicts.

# Channels

You can think in channels as queues, where you drop your data to take it out in the future. Go, has some block rules for different goroutines trying to write or read a channel at the same time. This mechanism it's a little bit tricky to understand, but we can think in a simple way: This mechanism just exists to asynchronous goroutines don't access empty memory spaces. For example, if you are trying to read a channel and there is no data in it, the goroutine will block until some data is written to the channel. If you are trying to write to a channel and there is no space in it, the goroutine will block until some data is read from the channel. This way, we can ensure that the data is always available when we need it, and that we don't have any conflicts when multiple goroutines are trying to access the same channel.

# Sync.Pool

Primarily used to manage a pool of objects that can be reused across multiple goroutines. This can help reduce the overhead of allocating and deallocating memory for frequently used objects, improving performance and reducing memory fragmentation.
The sync.Pool type provides a way to create a pool of objects that can be reused across multiple goroutines. It allows you to define a function that creates new objects when the pool is empty, and it automatically manages the lifecycle of the objects in the pool.

When use sync.Pool?

- When you have a large number of short-lived objects that are frequently created and destroyed.
- When you want to reduce the overhead of memory allocation and deallocation.
- When you want to improve performance by reusing objects instead of creating new ones.

When not use sync.Pool?

- When you have a small number of long-lived objects that are not frequently created and destroyed.

# [rate.limiter standard library](https://go.dev/wiki/RateLimiting)

The rate limiter is a concurrency primitive that allows you to control the rate at which requests are sent to a resource. It is useful for preventing overloading a resource or for implementing rate limiting in APIs. The rate limiter can be used to limit the number of requests sent to a resource over a specific period of time, such as 10 requests per second.

# [Testing concurrent code with testing/synctest](https://go.dev/blog/synctest)
