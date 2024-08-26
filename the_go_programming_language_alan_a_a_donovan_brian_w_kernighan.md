# The Go programming language, Alan A A. Donovan, Brian W Kernighan


### Intro
- Rob Pike qoute **complexity is multiplicative**
- definig a standalone package you have to define the package as `package main`

```go
package main

func main() {
    
}
```

- `strings.Join()` is faster than loops
```go
package main 

func main(){
    fmt.Println(strings.Join(os.Args[1:], " "))
}

```

TRY TO ANSWER QUESTION 1.3 PAGE 8

