# [19. Rob Pike - What We Got Right, What We Got Wrong | GopherConAU 2023](https://www.youtube.com/watch?v=yE5Tpp2BSGw)

# [Text Reference](https://commandcenter.blogspot.com/2024/01/what-we-got-right-what-we-got-wrong.html)

```

The Golang was built to be more than just a programming language, was built to make production software easier and more productive

Rob Pike
```

# the outstanding decision about Golang

## Golang specification

Indeed, you have a simple place, where all details about the language are gathered

- [go spec](https://go.dev/ref/spec)

## Other minor aspects of the specification

- Compatibility across version
- Run and build binaries across different OS
- The Library
- Tooling 😍
    - This is really outstanding in Golang. Things like
        - gofmt
        - go vet
        - go test
        - go run
        - go build
        - go install
        - go get
        - go mod
            - tidy
            - init
        - go tidy

        make your life easier and tons of time saved
