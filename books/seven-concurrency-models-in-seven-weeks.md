[Seven Concurrency Models in Seven Weeks: When Threads Unravel]

```
If you've spent hours wrestling with difficult-to-diagnose threading bugs, it might be hard to believe, but a concurrency solution can be simpler and clearer than its sequencial equivalent when written in the right language with the right tools.

Paul Butcher
```

## About the Author

Have PhD in languages for parallel and distributed computing

# The book objective

I like authors that try to be clear of what is the objective of the work that they are putting in the lines, some don't have this carefully, and in my opinion, don't write very assertive books. Paul surprise me at the begginig.

Butcher starts by arguing the way that Hardware, software complexity, concurrency-driven, multicore architecture, and functional programming languages would begin to emerge and would shape the way we program.

And that in the end we will cringe after know, how thread-lock, the widely used concurrency solution works, besides that, the objective of the book, is to make the reader auto sufficient to be able to choose the right concurrent model for solving specific problems.

> The primary driver behind this resurgence of interest is what's becode known as the "multicore crisis." Moore's law continus to deliver more transistor per chip, but instead of those transistors being used to make a single CPU faster, we'er seeing computers with more and more cores.
> ...
> These days if you need to exploit mulitple cores, and that means exploiting parallelism. - Paul Butcher

## Related but different

If you already search for concurrency on Google, will be possible to see a lot of examples around the difference between parallelism and concurrency, most example love to quote the famous Rob Pike phrase:

```
Concurency is about dealing with lots of things at once. Parallelism is about lots of things at once.
```

But the author gives an example that I like a lot:

The work that our mother does, pays attention to a lot of things while she is trying to made food, we can call of `concurrency mother work`, if you have two mothers, doing the same work, then it can be called a `parallelism mother work`

**What the parallelism and concurrency have in common?**

Both go beyond the traditional sequential programming model in which things happen one at a time, one after other.

## Nondeterministic / Deterministic

Concurrent programs are often `non-deterministic`, they will give different results depending on the precise timing of events. If you're working on a genuinely concurrent problem, `non-determinism` is natural and to be expected. Parallelism, by contrast, doesn't necessarily imply `non-determinism`.

# Threads and Locks

```
Threads-and-locks programming is like a Ford Model T. It will get you from point A to point B, but it is primitive, difficult to drive, and both unreliable and dangerous comparad to newer technology
```

I wil try to resume just the pitfalls of this model

## Mutual exclusion

This behavior it's about trying to use the threads-lock model to ensure that only one thread can access data at a time, this behavior can go wrong, including *race conditions* and *deadlocks*
