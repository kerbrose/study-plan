### Chapter 2: A Tour of Rust


- usage of `cargo` in various ways
```bash
cargo new hello
cargo new hello --vsc none
cargo run
cargo test
cargo update -p chrono --precise 0.4.29
```

- in rust function you could use `return` statment or you could leave it with an expression without semicolon.
```rust
n foo() -> i32 {
    42
}
// could be as following
fn foo() -> i32 {
    return 42;
```
another example
```rust
fn gcd(mut n: u64, mut m: u64) -> u64 {
    assert!(n != 0 && m != 0);
    while m != 0 {
        if m < n {
            let t = m;
            m = n;
            n = t;
        }
        m = m % n;
    }
    return n;
}
// or 
fn gcd(mut n: u64, mut m: u64) -> u64 {
    assert!(n != 0 && m != 0);
    while m != 0 {
        if m < n {
            let t = m;
            m = n;
            n = t;
        }
        m = m % n;
    }
    n
}
```

- writing tests in the same file using attribute `#[test]`
```rust
#[test]
fn test_gcd() {
    assert_eq!(gcd(10, 15), 5);
    assert_eq!(gcd(14, 15), 1);
    assert_eq!(gcd(2 * 3 * 5 * 11 * 17, 3 * 7 * 11 * 13 * 19), 3 * 11);
}
```

- rust raw string example
```rust
let a =  r#"
            <title>GCD Calculator</title>
            <form action="/gcd" method="post">
            <input type="text" name="n"/>
            <input type="text" name="m"/>
            <button type="submit">Compute GCD</button>
            </form>
        "#
// or 
let b =  r##"
            <title>GCD Calculator</title>
            <form action="/gcd" method="post">
            <input type="text" name="n"/>
            <input type="text" name="m"/>
            <button type="submit">Compute GCD</button>
            </form>
        "##
```

- rust function are thread-safe. // need to validate this.



### Chapter 3: Fundamental Types

- page 49, `type inference` in rust
