# Computer Science is Doomed

Computer science has always been about one thing:

> Translating **ideas** into **programs**.

CS is the study of how to take a problem and map it onto instructions that can be executed by a Von Neumann machine.

Behind this idea of **translating**, we assume that humans are the ones who will be responsible for implementing, maintaining and understanding these programs.

But with the growth of the LLMs, we can start to assume that this idea could not be 100% true.

---

Let's analyze the below examples, all of them do the same algorithm, try to find a palindrome, using Fortran, Lisp, Prolog, Perl, Python and Golang. Is it easy to understand that? It takes like 50 years to evolve the whole of computer Science, we create a lot of abstractions upon abstractions, but a simple thing like decreasing the complexity in the grammatical experience using a programming language remains the same. In the end, it's hard to understand, and you need to put some effort into all of those examples to answer: "what this programming are performing?"

<details>
<summary>Language examples 50 years</summary>

## Fortran

```Fortran
program Palindrome
    character(len=100) :: string
    integer :: i, len
    logical :: is_palindrome

    print *, "Enter a string: "
    read(*, '(A)') string

    len = len_trim(string)
    is_palindrome = .true.

    do i = 1, len/2
        if (string(i:i) /= string(len-i+1:len-i+1)) then
            is_palindrome = .false.
            exit
        end if
    end do

    if (is_palindrome) then
        print *, "The string is a palindrome."
    else
        print *, "The string is not a palindrome."
    end if
end program Palindrome
```

## Lisp

```Lisp
(defun palindrome-p (string)
  (string= string (reverse string)))

(let ((input (read-line)))
  (if (palindrome-p input)
      (format t "The string is a palindrome.~%")
      (format t "The string is not a palindrome.~%")))
```

## Prolog

```Prolog
(defun palindrome-p (string)
  (string= string (reverse string)))

(let ((input (read-line)))
  (if (palindrome-p input)
      (format t "The string is a palindrome.~%")
      (format t "The string is not a palindrome.~%")))
```

## Perl

```Perl
sub is_palindrome {
    my $string = shift;
    return $string eq reverse $string;
}

print "Enter a string: ";
my $input = <STDIN>;
chomp $input;

if (is_palindrome($input)) {
    print "The string is a palindrome.\n";
} else {
    print "The string is not a palindrome.\n";
}

```

## Python

```python
def is_palindrome(string):
    return string == string[::-1]

input_string = input("Enter a string: ")

if is_palindrome(input_string):
    print("The string is a palindrome.")
else:
    print("The string is not a palindrome.")
```

## Golang

```golang
package main

import (
    "fmt"
    "strings"
)

func isPalindrome(s string) bool {
    for i := 0; i < len(s)/2; i++ {
        if s[i] != s[len(s)-i-1] {
            return false
        }
    }
    return true
}

func main() {
    var input string
    fmt.Print("Enter a string: ")
    fmt.Scanln(&input)

    if isPalindrome(strings.ToLower(input)) {
        fmt.Println("The string is a palindrome.")
    } else {
        fmt.Println("The string is not a palindrome.")
    }
}
```

</details>


# What do we have now?

We can type some instructions into our computers, using natural language, and these models will spit some results. Yep, sometimes the results are completely wrong, but the average it's fair!

The difference in the evolutions of the syntax that I mentioned above, is that now, we can type:

> Using, Perl, Lisp, Fortran, Python and Golang, generate an algorithm that find a Palindrome

Easy to understand, no?


# Then, are we in trouble (developers)?

If we consider that the industrial revolutions always push new ideas to minimize human work and increase the output, whoever the output is. Then yes! it's part of human evolution, creating new approaches to minimize effort. Now, instead of reading the whole documentation, and passing hours trying to debug some syntax problem, or something very specific in the API, library or language that you are using, you can pass this task, for some model, that will read the documentation and show the step-by-step, sounds great, no?

But this tiny performance evolution has shown that we can replace a lot of developers with machines.
