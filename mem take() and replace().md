==Mem::take()== allows for you to take ownership of a field from an [[Enum]] into another. 

Rust does not allow for taking a field by value without replacing what was originally there; Mem::take() replaces the original field with its [[Default trait |Default]] value.

==Mem::replace()== does something similar, but allows for putting a specific value in the field it takes from.
##### Example
```rust
use std::mem;

enum MultiVariateEnum {
    A { name: String },
    B { name: String },
    C,
    D,
}

fn swizzle(e: &mut MultiVariateEnum) {
    use MultiVariateEnum::*;
    *e = match e {
        // Ownership rules do not allow taking `name` by value, but we cannot
        // take the value out of a mutable reference, unless we replace it:
        A { name } => B {
            name: mem::take(name),
        },
        B { name } => A {
            name: mem::take(name),
        },
        C => D,
        D => C,
    }
}

```