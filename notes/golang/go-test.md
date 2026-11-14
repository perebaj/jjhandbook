## Mocks for unit tests

### Ways to do it

* 1) Simulate the operations of a database using some data structure
* 2) Stubs: Objects that imitate the behavior of a real component
* 3) You can see in some blog posts that some tools actually simulated a database, or even spin up a parallel database to run the tests, but since we are talking about unit tests, approaches 1 and 2 are more plausible.

w
Usually option 1 is much more laborious and adds a lot of extra code to your tests; these operations can get so complex that you may stop trusting your own tests

## Table tests

You just pass our parameters and the expected returned value

```
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b, want int
    }{
        {2, 3, 5},
        {-1, 1, 0},
        {0, 0, 0},
        {100, -100, 0},
    }
    for _, tt := range tests {
        got := Add(tt.a, tt.b)
        if got != tt.want {
            t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.want)
        }
    }
}
```