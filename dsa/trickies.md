# Trickiest

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
