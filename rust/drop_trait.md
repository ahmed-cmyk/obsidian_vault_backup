# Rust Drop Trait

The `Drop` trait allows you to customize what happens when a value is about to go out of scope. This is where you'd put cleanup code, like releasing file handles or network connections.

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created.");
    // c and d are dropped when they go out of scope
}
```

- **Automatic Dropping**: Rust automatically calls `drop` when a value goes out of scope.
- **Preventing Manual `drop`**: You cannot explicitly call the `drop` method (e.g., `c.drop()`). This is to prevent double-free errors. If you need to force a value to be dropped early, use `std::mem::drop`.