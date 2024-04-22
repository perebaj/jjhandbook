# What is Security Engineering? Part 1.

# 1. Myths and misconceptions about security engineering

## The responsibility of security is only of the security team

The same occurs for DevOps Teams, QA teams and others. All these problems should be tackled by the whole team but guided by the specialists, not deliver all the responsibility to them.

## Security through obscurity

[Resource](https://en.wikipedia.org/wiki/Security_through_obscurity)

A really good example of this is trying to create completely no-sense endpoint names, like for routes that you don't want to create authentication, this just increases the comprehension, but doesn't guarantee any problem related to sec.

## Once secure,  always secure

Things that increase the security over time:

- Accumulation of technical debt
- Using deprecated components and libraries
- Outdated dependencies
- Zero-day exploits: very tricky to solve, because there are vulnerabilities that haven't been discovered yet, it's the first occurrence of the problem.

# The history

History holds the evolution of the internet by itself, the 1990s marked the beginning of the internet protocols.

The 2000s marked the popularity of web applications, so techniques like the below became popular:

SQL Injection
Cross-Site Scripting
Buffer overflow

In the 2010s with the rise of cloud computing, containerization, and microservices, the security problems became more complex and consequently new attacks rose over time.
