Borrowing an owned type as the argument for a function makes the function inflexible as opposed to just using a borrowed type directly. 

Borrowing an owned type prevents the function from being able to take borrowed types as input, but the reverse is not true.

Functions are able to convert borrowed owned types into borrowed types easily.

Using borrowed types avoids layers of indirection (having to use * to access the actual data).

##### Recommendations
- &str over &String
- &\[T\] over &Vec<T>
- &T over &Box<T>

##### Example
```rust
fn three_vowels(word: &String) -> bool {
    let mut vowel_count = 0;
    for c in word.chars() {
        match c {
            'a' | 'e' | 'i' | 'o' | 'u' => {
                vowel_count += 1;
                if vowel_count >= 3 {
                    return true;
                }
            }
            _ => vowel_count = 0,
        }
    }
    false
}

fn main() {
    let ferris = "Ferris".to_string();
    let curious = "Curious".to_string();
    println!("{}: {}", ferris, three_vowels(&ferris));
    println!("{}: {}", curious, three_vowels(&curious));

    // This works fine, but the following two lines would fail:
    // println!("Ferris: {}", three_vowels("Ferris"));
    // println!("Curious: {}", three_vowels("Curious"));
}
```
