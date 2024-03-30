# Radical Golang Tricks

## Sorting

Sorting structures in Golang was a really good experience, even with multiple fields, you can sort them easily. Here is an example:

```go
type Person struct {
    Name string
    Age  int
}

type People []Person

func (p People) Len() int {
    return len(p)
}

func (p People) Less(i, j int) bool {
    if p[i].Age == p[j].Age { // if the age is the same, sort by name, otherwise, sort by age
        // where i is the current index and j is the next index
        return p[i].Name < p[j].Name
    }

    return p[i].Age < p[j].Age
}

// Swap is used to swap the elements in the slice
func (p People) Swap(i, j int) {
    p[i], p[j] = p[j], p[i]
}

func main() {
    people := People{
        {"Alice", 25},
        {"Bob", 30},
        {"Charlie", 25},
        {"David", 20},
    }

    // we can user the sort.Sort passing a slice of the struct, because we implement our version of the sort.Interface
    // Preatty cool, right?
    sort.Sort(people)

    fmt.Println(people)
}
```

## Sum of digits

Sometimes you need to previously know some specific tricks to solve a problem, here are some of them:

- Sum of digits: 258 : 2 + 5 + 8 = 15

Ways to implement that using Golang

```go
func sumOfDigits(n int) int {
    sum := 0
    for n > 0 {
        sum += n % 10
        n /= 10
    }
    return sum
}


// or

func sumOfDigits(n string) int {
    sum := 0
    for _, c := range n {
        sum += int(c - '0')
    }
    return sum
}

// or

str := strconv.Itoa(n)

sum := 0
for _, c := range str {
    value, _ := strconv.Atoi(string(c))
    sum += value
}

```
