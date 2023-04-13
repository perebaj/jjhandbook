By convention, the package name is the same as the last element of the import path. For instance, the "math/rand" package comprises files that begin with the statement package rand.


[Golang declaration sintax](https://go.dev/blog/declaration-syntax) 


[Go-Tour](https://go.dev/tour/list)

# [varaibles and funcions](#Packages-variables-and-functions) | [Flow Control](#flow-control-statements-for-if-else-switch-and-defer) | [Methods and interfaces](#methods-and-interfaces)

## Packages, variables, and functions.

```
Functions continued
When two or more consecutive named function parameters share a type, you can omit the type from all but the last.

In this example, we shortened

x int, y int
to

x, y int

```
---

### Named return values
Go's return values may be named. If so, they are treated as variables defined at the top of the function.

These names should be used to document the meaning of the return values.

A return statement without arguments returns the named return values. This is known as a "naked" return.

Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

```
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

```
---

### Variables
The var statement declares a list of variables; as in function argument lists, the type is last.

A var statement can be at package or function level. We see both in this example.

```
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

---

### Variables with initializers
A var declaration can include initializers, one per variable.

If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

```
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}

```

---

### Short variable declarations
Inside a function, the := short assignment statement can be used in place of a var declaration with implicit type.

Outside a function, every statement begins with a keyword (var, func, and so on) and so the := construct is not available.

---

### Zero values
Variables declared without an explicit initial value are given their zero value.

The zero value is:

0 for numeric types,
false for the boolean type, and
"" (the empty string) for strings.


---

### Constants

Constants are declared like variables, but with the const keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the := syntax.

---

## Flow control statements; for, if, else, switch and defer

### For
Go has only one looping construct, the for loop.

The basic for loop has three components separated by semicolons:

- the init statement: executed before the first iteration
- the condition expression: evaluated before every iteration
- the post statement: executed at the end of every iteration

The init statement will often be a short variable declaration, and the variables declared there are visible only in the scope of the for statement.

The loop will stop iterating once the boolean condition evaluates to false.

### Forever

Forever
If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.

```
package main

func main() {
	for {
	}
}

```

### If


If
Go's if statements are like its for loops; the expression need not be surrounded by parentheses ( ) but the braces { } are required.

```
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}

```

### If and else
Variables declared inside an if short statement are also available inside any of the else blocks.

(Both calls to pow return their results before the call to fmt.Println in main begins.)

```
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

### Switch

Switch
A switch statement is a shorter way to write a sequence of if - else statements. It runs the first case whose value is equal to the condition expression.

Go's switch is like the one in C, C++, Java, JavaScript, and PHP, except that Go only runs the selected case, not all the cases that follow. In effect, the break statement that is needed at the end of each case in those languages is provided automatically in Go. Another important difference is that Go's switch cases need not be constants, and the values involved need not be integers.

```
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

### Defer
A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

### Creating a slice with make

Slices can be created with the built-in make function; this is how you create dynamically-sized arrays.

The make function allocates a zeroed array and returns a slice that refers to that array:

```golang
a := make([]int, 5)  // len(a)=5
```
To specify a capacity, pass a third argument to make:

```golang
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```


### Appending to a slice
It is common to append new elements to a slice, and so Go provides a built-in append function. The documentation of the built-in package describes append.

func append(s []T, vs ...T) []T
The first parameter s of append is a slice of type T, and the rest are T values to append to the slice.

```golang
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s)

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s)

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

```

### Range
The range form of the for loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

```golang
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

### Mutating Maps
Insert or update an element in map m:

```m[key] = elem```
Retrieve an element:

```elem = m[key]```
Delete an element:

```delete(m, key)```
Test that a key is present with a two-value assignment:

```elem, ok = m[key]```
If key is in m, ok is true. If not, ok is false.

If key is not in the map, then elem is the zero value for the map's element type.

Note: If elem or ok have not yet been declared you could use a short declaration form:

```go
package main

import "fmt"

func main() {
	m := make(map[string]int)

	m["Answer"] = 42
	fmt.Println("The value:", m["Answer"])

	m["Answer"] = 48
	fmt.Println("The value:", m["Answer"])

	delete(m, "Answer")
	fmt.Println("The value:", m["Answer"])

	v, ok := m["Answer"]
	fmt.Println("The value:", v, "Present?", ok)
}

```

## Methods and Interfaces

Go does not have classes. However, you can define methods on types.

A method is a function with a special receiver argument.

The receiver appears in its own argument list between the func keyword and the method name.

In this example, the Abs method has a receiver of type Vertex named v.

```
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

> ⚠️ You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int).

### Pointer receivers 

```
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs())
}
```

It's diferente for :

```
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

### Pointer receiver X Value

Use a value receiver when:

- You are working with small structs. If the struct is small, it may be more efficient to pass it by value, as the cost of copying the struct will be negligible.

- You do not need to modify the struct. If your method only needs to read from the struct and does not modify it, a value receiver is appropriate.

- You want to ensure that the struct is not modified. By using a value receiver, you are ensuring that the method cannot modify the original struct, which can be useful in situations where you want to ensure the struct remains unchanged.

Use a pointer receiver when:

- You are working with large structs. If the struct is large, passing it by value can be expensive. By using a pointer receiver, you can avoid the cost of copying the struct.

- You need to modify the struct. If your method needs to modify the struct, a pointer receiver is required, as a value receiver will only modify a copy of the struct.

- You want to modify the struct in a way that persists outside the method. By using a pointer receiver, you can modify the original struct and those changes will be visible outside the method.