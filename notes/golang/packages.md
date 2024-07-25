# Golang Layout

First, Golang doesn't have a standard layout. This was affirmed by Russ Cox in a [thread](https://github.com/golang-standards/project-layout/issues/117), so probably the best way to organize is to think before starting coding.

The Style that I usually like to follow up:

```
The package strategy that I use for my projects involves 4 simple tenets:

- Root package is for domain types
- Group subpackages by dependency
- Use a shared mock subpackage
- Main package ties together dependencies
```

# Important Resources

- [Standard Package Layout](https://medium.com/@benbjohnson/standard-package-layout-7cdbc8391fc1)
- [Style Guideline for Go Packages](https://rakyll.org/style-packages/)
- [Ben Jhonson - WTF](https://github.com/benbjohnson/wtf?tab=readme-ov-file)
- [rsc - this is not a standard go project layout](https://github.com/golang-standards/project-layout/issues/117#issue-854742264)
- [organizing go code](https://go.dev/blog/organizing-go-code)
- [simple go layout](https://eli.thegreenplace.net/2019/simple-go-project-layout-with-modules/)

- [reddit post](https://www.reddit.com/r/golang/comments/1ak3mmm/internal_package/)
