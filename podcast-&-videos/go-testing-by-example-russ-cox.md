# [Go Testing By Example (GopherCon Australia 2023)](https://www.youtube.com/watch?v=X4rxi9jStLo&t=360s)

```
Testing is a determined, systematic attempt to break a program tha you think is working.
```

```
Testing your program once make sure it works today

Automaeted, continuous testing makes sure it keeps working(despite time an other people).
```

## What makes tests good?

**Hard work, attention and time**

# Tips

* Make it easy to add new test cases.
* Use test coverage to find untested code.
    - go test -coverprofile=cover.out
* Coverage is no substitute for thought.
* Write exhaustive tests.
* Separate test cases from test logic.
* Look for special cases
* If you didn't add a test, you didn't fix the bug.
* Not everything fits in a table.
* Test cases can be in test data files.
* Makes test errors readable.
* If the answer can change, write code to update them

    Imagine that you want to create a file to verify if the output is correct, you can do that using the diffJSON function, it's ok. But now you want to save the output in a file. To enable that, you can use the -update flag to update the file with the new output.

    ```bash

        go test -update

        var update = flag.Bool("update", false, "rewrite testdata/*json files")
        if *update {
            // update the file
        }

    ```

* txtar for multiple files
* Code quality is limited by test quality



# Txt Data

Here Russ gives some gold tips about how to deal with tests that need to read data from files. The first tip is to keep the tests in two different files, one for the input and another for the expected output.

**Steps:**

- Read the file
- Run the converter
- Check with the expected output. Out can use diffJSON to compare the two files.

In the process of calculating the diff between those tests, it's a good practice to implement the **diffJSON** that will create a more readable output and facilitate the debugging process.


## Libraries
- https://pkg.go.dev/golang.org/x/tools/txtar#Parse
- https://pkg.go.dev/cmd/test2json
- https://github.com/rsc/script

# Resources

[testing - Russ Cox](https://research.swtch.com/testing)
