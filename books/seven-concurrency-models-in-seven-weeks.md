[Seven Concurrency Models in Seven Weeks: When Threads Unravel](https://www.amazon.com/Seven-Concurrency-Models-Weeks-Programmers/dp/1937785653)

```
If you've spent hours wrestling with difficult-to-diagnose threading bugs, it might be hard to believe, but a concurrency solution can be simpler and clearer than its sequential equivalent when written in the right language with the right tools.

Paul Butcher
```

## About the Author

Have PhD in languages for parallel and distributed computing

# The book objective

I like authors that try to be clear of what is the objective of the work that they are putting in the lines, some don't have this carefully, and in my opinion, don't write very assertive books. Paul surprise me at the begging.

Butcher starts by arguing the way that Hardware, software complexity, concurrency-driven, multi core architecture, and functional programming languages would begin to emerge and would shape the way we program.

And that in the end we will cringe after know, how thread-lock, the widely used concurrency solution works, besides that, the objective of the book, is to make the reader auto sufficient to be able to choose the right concurrent model for solving specific problems.

> The primary driver behind this resurgence of interest is what's become known as the "multi core crisis." Moore's law continue to deliver more transistor per chip, but instead of those transistors being used to make a single CPU faster, we'er seeing computers with more and more cores.
> ...
> These days if you need to exploit multiple cores, and that means exploiting parallelism. - Paul Butcher

## Related but different

If you already search for concurrency on Google, will be possible to see a lot of examples around the difference between parallelism and concurrency, most example love to quote the famous Rob Pike phrase:

```
Concurrency is about dealing with lots of things at once. Parallelism is about lots of things at once.
```

But the author gives an example that I like a lot:

The work that our mother does, pays attention to a lot of things while she is trying to made food, we can call of `concurrency mother work`, if you have two mothers, doing the same work, then it can be called a `parallelism mother work`

**What the parallelism and concurrency have in common?**

Both go beyond the traditional sequential programming model in which things happen one at a time, one after other.

## Nondeterministic / Deterministic

Concurrent programs are often `non-deterministic`, they will give different results depending on the precise timing of events. If you're working on a genuinely concurrent problem, `non-determinism` is natural and to be expected. Parallelism, by contrast, doesn't necessarily imply `non-determinism`.

# Threads and Locks

```
Threads-and-locks programming is like a Ford Model T. It will get you from point A to point B, but it is primitive, difficult to drive, and both unreliable and dangerous compared to newer technology
```

I wil try to resume just the pitfalls of this model

## Mutual exclusion

This behavior it's about trying to use the threads-lock model to ensure that only one thread can access data at a time, this behavior can go wrong, including *race conditions* and *deadlocks*

We avoid this through locks, which can be held by only a single thread at a time

# Perils of threads and locks

- Race conditions 
- deadlock
- memory visibility

# Functional 

> Functional programming is like an electric car, advanced, futuristic and not yet widely used, but it's what we'll all rely on in twenty years.

## Terms 

**Mutable state**

A state is basically a variable assignment, and we can slip team in different types, like, mutable, if this **state** can be modified from different parts of the code 

## Perils of mutable state

These are some of the dangers of mutable state that inflict concurrent programs and what functional programming enables, is basically avoid this perils

**Hidden mutable state**

The tease here is about variables that affect the behavior of your code, but it's hard/impossible to avoid it when it's happening, you could say that this is a bug, but in this case, I think that doesn't matter, the problem is that some languages enable it, turning easy to commit dumb mistakes and crash your codebase.


**Escapologist/Escaped mutable state** ->

Escapology is the practice of escaping from restraints or traps, in this case, we could say that the code looked thread-safe, maybe you had some functions that when called by a different thread, could trigger a concurrent modification exception, even if this isn't clear in the code.
1118
