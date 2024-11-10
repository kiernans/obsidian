Developers care to have objects be created and destroyed in controlled way as it allows for predictable programs. 

Finalisation is the ability to control how cleanup occurs when an object is being destroyed. 

Important when dealing with files and network sockets (close them properly on failure or when done with them).

==Do not rely on Finalisation to always run. Will not run if function crashes before exit, in a panic of an already panicking thread, and if in an infinite loop==
##### Example
```rust
fn bar() -> Result<(), ()> {
    // These don't need to be defined inside the function.
    struct Foo;

    // Implement a destructor for Foo.
    impl Drop for Foo {
        fn drop(&mut self) {
            println!("exit");
        }
    }

    // The dtor of _exit will run however the function `bar` is exited.
    let _exit = Foo;
    // Implicit return with `?` operator.
    baz()?;
    // Normal return.
    Ok(())
}
```