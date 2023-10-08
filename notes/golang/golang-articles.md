[An introduction to handler and servermuxes in go](https://www.alexedwards.net/blog/an-introduction-to-handlers-and-servemuxes-in-go)


---

[Interfaces exaplained](https://www.alexedwards.net/blog/interfaces-explained)

* An interface type in Go is kind of like a definition. It defines and describes the exact methods that some other type must have.

Why are they useful?
There are all sorts of reasons that you might end up using a interface in Go, but in my experience the three most common are:

- To help reduce duplication or boilerplate code.
- To make it easier to use mocks instead of real objects in unit tests.
- As an architectural tool, to help enforce decoupling between parts of your codebase.

---
* [Flexible mocking for testing in go](https://medium.com/safetycultureengineering/flexible-mocking-for-testing-in-go-f952869e34f5)

## The Anti-Pattern
the hidden problem is that in order to test the two functions above we need to mock the **entire Store interface**, even when we are only using a sub set of the functions (some method bodies ignored for brevity):

- [Interfaces good pattern](https://github.com/golang/go/wiki/CodeReviewComments#interfaces)

```go
type Store interface {
    ListUsers() ([]*User, error)
    ListTeamsByUser(userID int) ([]*Team, error)
    ListAddressesByUser(userID int) (*[]Address, error)
    // ...etc
    GetUser(id int) (*User, error)
    GetTeam(id int) (*Team, error)
    // ...etc
    CreateUser(user *User) (int, error)
    CreateTeam(userID int, team *Team) (int, error)
    // ...etc
    SetUserDefaultAddress(userID int, addressID int) error
    // ...etc
}
type store struct {
    db sql.DB
}
func NewStore(conn string) Store {
    db := initDB(conn)
    return &store{db:db}
}
```


---
[Test Principles](https://medium.com/@kentbeck_7670/programmer-test-principles-d01c064d7934)

Constraits to get start with tests.

* Tests are an oracle.
* Programmer tests should be deterministic.
* I liked the Facebook policy of simply deleting non-deterministic tests.
* Programmer tests should be predictive. If your oracle says, “Go ahead, deploy,” and deployment fails, you’ll stop believing your oracle.
* Programmer tests **should be sensitive to behavior changes and insensitive to structure changes.**
* Programmer tests should be cheap to write.(**We don’t get paid for tests, we get paid for code that**)
* Programmer tests should be cheap to read.
---
[GopherCon 2017: Mitchell Hashimoto - Advanced Testing with Go
](https://www.youtube.com/watch?v=8hQG7QlcLBk&ab_channel=GopherAcademy)

## Test Fixtures

* Use relative path "test-fixtures" directory as a place to store test data
* Very useful for loading config, model data, binary data, etc

## Gondel Files

* Test complex output without manually hardcoding  it
* Human eyeball the generated golden data. If it is correct, commit it.
* Very scalable way to test comples structures 


## [Go pointers: When & How to use them Efficiently](https://www.youtube.com/watch?v=3WsEDZRif6U) 

Good content about golang pointers

- Sintex sugar compared with C
- each pointer = 8 bytes
- If you don't use pointer to manipulate your structures, the size of memory is the size of your struct

---

# Readlist
* [Golang Layout](https://github.com/golang-standards/project-layout)
* [GO by example](https://gobyexample.com/)
* [GO Training](https://github.com/ardanlabs/gotraining/blob/master/reading/README.md)
* [Testing Http handler go](https://www.cloudbees.com/blog/testing-http-handlers-go)

---


