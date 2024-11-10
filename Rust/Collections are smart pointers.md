Using the [[Deref]] trait allows you to treat [[Collections | collections]] as [[Smart Pointers | smart pointers]], offering owning and borrowed views of the collections.

##### Example
```rust
use std::ops::Deref;

struct Vec<T> {
    data: RawVec<T>,
    //..
}

impl<T> Deref for Vec<T> {
    type Target = [T];

    fn deref(&self) -> &[T] {
        //..
    }
}
```
The above example shows how to convert an owned type (Vec<T>) into a borrowed type  or slice (&[T]), allowing for functions created for Vec<T> to also work with &[T].

This seems to relate to [[Borrowed types for arguments]] as it is what happens behind the scenes?