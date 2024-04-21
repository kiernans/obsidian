This idiom is just for adding strings together in a readable way.

Disadvantage is that this approach is less efficient than using pushes to concatenate strings.



##### Example
```rust
fn say_hello(name: &str) -> String {
    // We could construct the result string manually.
    // let mut result = "Hello ".to_owned();
    // result.push_str(name);
    // result.push('!');
    // result

    // But using format! is better.
    format!("Hello {name}!")
}

```
