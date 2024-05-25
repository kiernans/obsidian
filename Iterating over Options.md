I guess the tip is that you can use Options anywhere that using iterators as it  implements the Into Iterator trait? I'm unsure of the usefulness though...
##### Example
```rust
let turing = Some("Turing"); 
let mut logicians = vec!["Curry", "Kleene", "Markov"]; 
logicians.extend(turing); 
// equivalent to 
if let Some(turing_inner) = turing 
{ 
logicians.push(turing_inner); 
}
```