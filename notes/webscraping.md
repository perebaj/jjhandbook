# Web Scraping

## Introduction

My first duty as a software developer was to create scrapers, and it was awesome to see how distributed systems work, how to manage a lot of data, and how the difficulty increases when you need to manage different sources, formats and so on. In this note, I will try to explain the perils & benefits, some architecture possibilities, and some tips to manage this kind of system.

Obs: All opinions are my own, based on my experience. I probably will use it to keep my brain fresh about this topic and also to help me to remember some important points.

## Architecture Perils

When you are creating a scraper in scale, there are some perils that you need to be aware of, and some of them are:

- **Disposition of the crawlers**: Different sources, formats, data and so on, but all the code seems very similar, what is the best way to manage it? A monolithic code? A microservice? A library? A framework?

Here are some points that you need to think about, like, What is more important for you?

Don't have any kind of dependency between the crawlers? **If a crawler B fails, for a new feature, the crawler A should NEVER be affected by it?**
If the answer is **yes**, probably, creating an isolated service for each crawler is the best option. You could have a third library to manage the common parts, but the main code, responsible for gathering the business logic, should be isolated. In this case, you could also use your imagination to the amount of code that will be repeated, like the Docker file, the CI/CD, the tests, and so on, and conforming the things grow, decrease the complexity with support libraries.

**Crawler C should have a different **resource allocation strategy** than crawler** D? Like, use more memory, or more CPU, if the answer is yes**, you should probably have a different service for each crawler.

I'm not reinventing the wheel, just reasoning about microservices and monolithic code. To summarize, I like [this paper](https://dl.acm.org/doi/10.1145/3593856.3595909), which explains the pros and cons of each one:

**Pros of microservices:**

- Improve performance
    Separate binaries can be scaled independently
- Improve fault tolerance
    A failure in one service does not affect the others
- Improve the abstraction boundaries
    Each service can be developed, tested, and deployed independently
- Flexibility Rollout
    Different binaries can be updated at different times

**However, the cons are:**

- Increase complexity
    It hurts performance: The overhead of communication between services, serialization, and deserialization across a network can be significant
- It's hard to manage:
    Instead of a single binary to build, test, and deploy, you have many

    Obs: In this case, I am not sure if the monolithic suppresses this lack of complexity, but it's a good point to think about it.
- It freezes APIs
    It's hard to change the API of a service without breaking to forward and backward compatibility of the following services
- Sometimes it slows down the development
    When you have some change that affects multiple services, you need to coordinate the deployment of all of them

## Compare data

## Monitoring

Another important point is the monitoring of the crawlers, how create efficient alerts?

If we stop again to think about the most basic case, we know that the **input is a couple of URLs**, and the **output is a couple of JSONs**, in this case, we could use a simple monitoring system, comparing how much content was extracted from the URLs, create a dashboard and create a notification when the percentage was below of the expected.

Or, you can also create a calculation strategy on top of how many status codes each URL returns, and alerts when the percentage of status code != 200 is above the expected.

After defining the approach, you can use tools like Grafana and Prometheus to create the dashboards and alerts.

It's important to choose useful label layers for each metric, metric names, the unit of the metric, and so on, to create a good dashboard and alerts.

A good reference is the [Prometheus best practices](https://prometheus.io/docs/practices/naming/).

## Data storage

## If all goes wrong

If the crawler process stops for some disaster, you shouldn't lose the place where you stopped, and also, you should be able to restart the process from the point that you stopped.

https://github.com/eapache/go-resiliency/tree/main
https://dl.acm.org/doi/pdf/10.1145/2366316.2366331

## Streaming data and real-time processing

The arrow that connects the different scrapers with the data storage and the following services is the streaming services, like Kafka, RabbitMQ and Pub/Sub.

## Development of new scrapers

Initially, the development of multiple scrapers could turn into a nightmare, at least for me, what reduces this swamp, and the time that the developers spend trying to understand what is happening. This, could be circumvented by good documentation, so, before starting to code, or adapt some existent crawler, you should adapt or create the goals of the code, a simple input/output JSON it's enough.

## Conclusion
