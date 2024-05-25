Only granting [[mutability]] to process data and then make the data immutable after. Done by either putting data in a nested block or using variable rebinding.

##### Example
Using nested block:
```rust
let data = {
    let mut data = get_vec();
    data.sort();
    data
};

// Here `data` is immutable.
```

Using variable rebinding:
```rust
let mut data = get_vec(); 
data.sort(); 
let data = data;  // Here `data` is immutable.
```
