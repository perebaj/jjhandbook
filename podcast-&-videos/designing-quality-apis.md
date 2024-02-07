# [Designing Quality APIs (Cloud Next 18)](https://www.youtube.com/watch?v=P0a7PwRNLVU&ab_channel=GoogleCloudTech)

Briefing:
The main idea of this talk was that software is hard to change because it is complicated and permeated with assumptions, principally when you have integrations and versioning. Pay attention to the basics, in the beginning, to avoid messy software in the future

What is an API: Two pieces of software trying to communicate between network

The communication needs to be fast, secure, and easy to program

Software is hard to change:

- Because it is complicated
- Because it is permeated with assumptions

Some API styles offer an "attack" on important problems.

## 2 dominant styles of API

- Entity-oriented: my code manipulates the entities you expose

- Procedure-oriented: my code calls your procedure/function/method

## API properties to attack important problems

- Uniform API across all applications
- Uniform model for relationships
- Free of implementation assumptions

Unfortunately, HTTP tells you how to do CRUD, but not how to do query or versioning
