Leaktest
------

### Installation

Go 1.7+

```
go get -u github.com/gostores/leaktest
```

Go 1.5/1.6 need to use the tag `v1.0.0`, as newer versions depend on
`context.Context`. 

### Example

These tests fail, because they leak a goroutine

```go
// Default "Check" will poll for 5 seconds to check that all
// goroutines are cleaned up
func TestPool(t *testing.T) {
	defer leaktest.Check(t)()

    go func() {
        for {
            time.Sleep(time.Second)
        }
    }()
}

// Helper function to timeout after X duration
func TestPoolTimeout(t *testing.T) {
	defer leaktest.CheckTimeout(t, time.Second)()

    go func() {
        for {
            time.Sleep(time.Second)
        }
    }()
}

// Use Go 1.7+ context.Context for cancellation
func TestPoolContext(t *testing.T) {
    ctx, _ := context.WithTimeout(context.Background(), time.Second)
	defer leaktest.CheckContext(ctx, t)()

    go func() {
        for {
            time.Sleep(time.Second)
        }
    }()
}
```


LICENSE
------
Same BSD-style as Go, see LICENSE
