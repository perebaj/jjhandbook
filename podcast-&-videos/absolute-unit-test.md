# [Dave-Chaney Absolute Unit test](https://dave.cheney.net/2019/04/03/absolute-unit-test)

> - You are not allowed to write any production code unless it is to make a failling unit test pass;
>- You are not allowed to write any more of a unit test than is sufficient to fail, and compilation failures are failures;
>- You are not allowed to write any more production code than is sufficient to pass the one failling unit test
>
> Uncle Bob

> Dave Cheney
>
> - If i knew what I wanted to write, the writing the failing test first was easy
> - If i didn't know what I was writing, the cost of experimentation started at 200%; change the code, fix the test.
>
> - This cost went up and to the right when one function called another

In Golang the unit of code is the package.
Code coverage is your guide.
When you refactor, use coverage to tell you where you need to add tests.
