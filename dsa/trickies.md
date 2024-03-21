# Trickies

Sometimes you need to be able to know previously, some specific tricks to solve a problem, here are some of them:

- Sum of the integer for number like 25: the sum 2 + 5 = 7

To do that, you can conver the number to a string and then iterate over the string and sum the numbers.

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
