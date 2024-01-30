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

Crawler C should have a different deployment strategy than crawler D? Like, use more memory, or more CPU, if the answer is yes, you should probably have a different service for each crawler.

I'm not creating the wheel, just reasoning about microservices and monolithic code. To summarize, I like of [this paper](https://dl.acm.org/doi/10.1145/3593856.3595909), which explains the pros and cons of each one:

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

## Data storage
