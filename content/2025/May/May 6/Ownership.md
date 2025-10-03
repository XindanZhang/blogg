## Ownership
// Ownership is a set of rules that governs how a Rust program manages memory.

```rust
fn main() {
  let s = String::from("hello");
  print_str(&s);
  println!("s = {}", s);
  let m = &s;
  let s2 = s.clone(); // s is not moved, s2 is a copy. ✅ s is cloned, s2 owns a new copy, s still valid
  // if we use s2 = s; then s is moved to s2, and s is no longer valid
  // let s2 = s; // s is moved to s2, and s is no longer valid
  println!("m, s2 = {}, {}", m, s2);
  println!("original s = {}", s);
  print_str(&s);
}

fn print_str(s: &String) {
  println!("s1 = {}", s);
}

// println!("s = {}", s);
```
