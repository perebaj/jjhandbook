# [A Philosophy of Software Design | John Ousterhout | Talks at Google](https://www.youtube.com/watch?v=bmSAYlu0NcY&t=5s)

What is the most important thing in computer science: **problem decomposition**

How do you take a complicated problem or system and chop it up into pieces that you can build relatively independently? Basically how you do a good design decisions?

**The first** question around good software design is: **Is this could be taught?** Or is this something that you are born with? 100% sure that can be taught!

**Second.** Who is going to do it? The best guy to do it is the **software engineer**. Having a lot of different experiences, and having sure that you are evolving and taking notes about engineering decisions that you made in the past, comparing them and also learning from them. (And of course, have a good ecosystem of good engineers around you).

**Third.** How do you do it? Criticism!

# The magic secrets

## Deep classes

John saw classes as a payment process. Where the top of the class is the cost. (what are the things that you need to pay to use it?) and the middle is the benefit. What methods and functionalities does it provide?

And he also teases about shallow and deep classes. The worst way to build a class is to have a high cost with low benefits. Maybe you should question yourself if this class is really necessary or should be a simple function.

Someone somewhere creates this idea that methods and functions should be small, but defining a functionality of something by its size, it's an awkward way to think about software design. Let's think about the I/0 Unix interface.

```c
int open(const char *path, int flags, mode_t mode);
ssize_t read(int fd, void *buf, size_t count);
ssize_t write(int fd, const void *buf, size_t count);
int close(int fd);
off_t lseek(int fd, off_t offset, int whence);
```

In this case, you have a complex operation, but it's simple to use, just **five** well-defined methods!

Hidden below this simple interface:
- disk allocation
- directory management
- permission management

But for the end user, this doesn't matter, I just want to, open, read, write or close my file!

## Working code isn't enough

Tactical x Strategic design

Tactical is when your manager says to deliver a feature for yesterday, and maybe everyone already has this type of guy in your team/company, For me it's ok, but it's easy to notice that in the long term just can cause a mess on the code base and the product.


- Invest in good design today
    - Goal: Produce a great design
    - Simplify future development
    - Minimize complexity
    - Must sweat the details (small things)

## How to move fast

- Hire top programmers.

It's possible to have a successful product, with a spaghetti code base, but it's not sustainable, we have a lot of examples in the market (on both sides), but related to software design, have good programmers that will take over the design and make it better with handsome patterns and principles, it's the best way to move fast with a solid architecture.

Some basic "rules":

- Make continual small investments: 10-20% of your time
- When writing new code
    - **Careful** design
    - Good documentation

- When changing existing code
    - **Always** find something to improve

```txt
Small steps, not heroics
```
