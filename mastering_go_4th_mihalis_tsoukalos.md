# Mastering Go

## Chaperter 1

install go docs

```bash
go install golang.org/x/tools/cmd/godoc@latest
```

view documentation of a function

```bash
go doc fmt
```

running go package by

```bash
go build hello_world.go
go build -o final_exe hello_world.go
# or
go run hello_world.go
```

**Note:** if no source files are provided in `go build`, it looks for a main package in the current directory.

example of a global variable

```go
package main

var GlobalVariable int = 42


func main() {
    // Access the global variable from within the main function
    fmt.Println(GlobalVariable)
}
```

example of using `fallthrough` 
1. Shared Code Execution:
```go
package main
import "fmt"

func main() {
    number := 2

    switch number {
    case 1:
        fmt.Println("One")
        fallthrough
    case 2:
        fmt.Println("Two")
        fallthrough
    case 3:
        fmt.Println("Three")
    default:
        fmt.Println("is it a number")
    }
}

// output
// Two
// Three
```

2. Stateful Logic:
```go
switch state {
case "Start":
    initialize()
    fallthrough
case "Continue":
    process()
    fallthrough
case "Finish":
    cleanup()
}
```

traditional for loop
```go
package main
import "fmt"
func main(){
    for i:=0; i<10; i++{
        fmt.Print(i*i, " ")
    }
    fmt.Println()
}
```

traditional for loop (idimatic go)

```go
i := 0
for ok := true; ok; ok = (i != 10){
    fmt.Print(i*i, " ")
    i++
    fmt.Println()
}
```

**Note:** golang doesn't support `while`

iterating a slice in go

```go
aSlice := []int{-1, 2, 1, -1, 2, -2}
for i, v := range aSlice{
    fmt.Println("index: ", i, "value: ", v)
}
```

calling a function using `go` transform it as a goroutine

```go
package main

import (
    "fmt"
)

func main(){
    go fetch(url)
}

func fetch(url string){
    return url
}
```

logging in go

## Chaperter 2


printing the type of the variable in Go, using `Printf()` verbs `%T`.

```go
import(
    "fmt"
)
myNumber := 22
fmt.Printf("the type is:  %T", myNumber)
```

example of creating `arrays`

```go
first := [4]string{"zero", "one", "two", "three"}
second := [...]string{"zero", "one", "two", "three"}
```

example of 2D slice

```go
// example of a 2D slice
twoD := [][]int{{1, 2, 3}, {4, 5, 6}}
for _, i := range twoD {
    for _, k := range i {
        fmt.Print(k, " ")
    }
    fmt.Println()
}
```

accessing a memory address of a variable using `&`
```go
var f float64 = 12.123
fmt.Println("Memory address of f:", &f)
```

## Chaperter 3


creating a map in 2 ways

```go
first_map := make(map[string]int)
first_map["foo"] = 1
first_map["bar"] = 2

second_map := := map[string]int {
    "foo": 1
    "bar": 2
}
```

iterating over a map

```go
menu := map[string]float64{
    "eggs":    1.75,
    "bacon":   3.22,
    "sausage": 1.89,
}

for dish, price := range menu {
    fmt.Println(dish, price)
}
// or
for _, price := range menu {
    fmt.Println(price)
}
for dish := range menu {
    fmt.Println(dish)
}
```

in golang regex negative lookahead isn't supported for technical reasons, more in https://stackoverflow.com/questions/26771592/negative-look-ahead-in-go-regular-expressions

matching an expression could be as

```go
package main
import (
	"fmt"
	"regexp"
)
func main(){
    first := "AA"
    second := "Aa"
    third := "Apple"
    t1 := []byte(first)
    t2 := []byte(second)
    t3 := []byte(third)
    re := regexp.MustCompile(`^[A-Z][a-z]*$`)
    match1 := re.Match(t1)
    match2 := re.Match(t2)
    match3 := re.Match(t3)
    fmt.Println(match1)
    fmt.Println(match2)
    fmt.Println(match3)
}
```

