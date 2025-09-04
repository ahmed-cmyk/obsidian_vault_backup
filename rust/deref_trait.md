# Rust Deref Trait

The `Deref` trait allows you to customize the behavior of the dereference operator (`*`). By implementing `Deref`, your smart pointer can be treated like a regular reference, allowing you to call methods on the underlying type directly.

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

fn main() {
    let x = 5;
    let y = MyBox::new(x);

    assert_eq!(5, x);
    assert_eq!(5, *y); // Dereferencing MyBox<T> like a regular reference
}
```

- **Deref Coercion**: Rust performs deref coercion when it finds types that implement the `Deref` trait. This allows `&MyBox<String>` to become `&String`, and then `&String` to become `&str`, enabling method calls like `hello(&m)` where `m` is `MyBox<String>` and `hello` expects `&str`.