# Why rust in production?

## Reliability and stability = performance

In theory, more **reliable and stable software is easier to maintain and cheaper to operate.** Less time is spent on debugging and more on features. But yep, this could be used for every language, there is an overlap between this wide concept and what the language will offer for your team.

But seems that this overcontrols under the language(Rust), enables fewer bugs in the long term, at least in the testimonies of some companies like Mozilla, Microsft and so on.

Where is a graph that shows the relation between the bug cost and the stage in your application belt

![](./bug-costs.svg)

Also, the testimonies, argue that for larger applications, due to the strong type system. its focus on memory safety and some powerful tooling, were also some points that enable the "stability" and "reliability"


## Rust predictable runtime

This means that the service will run smoothly without unexpected peak of performance or latency

One of the lacks of Golang, is that Go's garbage collector acts in unpredictable ways putting some latency peaks in the applications

## Lower latency variance

## Cost saving

Services that need to scale for a huge number of requests also need to increase the infrastructure to handle it. If the same workload can be handled with fewer resources, it means that you are saving money.

## Really good resources

- [Why Discord is switching from Go to Rust](https://discord.com/blog/why-discord-is-switching-from-go-to-rust)
- [Rust NPM white paper](https://www.rust-lang.org/static/pdfs/Rust-npm-Whitepaper.pdf)
- [Sentry using rust in production -  A good repository reference](https://github.com/getsentry/relay)


