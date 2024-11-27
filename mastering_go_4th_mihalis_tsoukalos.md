# Mastering Go

## Chaperter 1

install go docs

```bash
go install golang.org/x/tools/cmd/godoc@latest
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
