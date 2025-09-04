# Rust Box Smart Pointer

`Box<T>` allows you to store data on the heap rather than the stack. This is useful when:
- You have a type whose size can't be known at compile time, but you need to use a value of that type in a context that requires a precise size.
- You have a large amount of data and want to transfer ownership without copying the data.
- You want to own a value and only care that it's a type that implements a particular trait rather than being a specific type.

### How to Use `Box<T>`

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};

fn main() {
    let b = Box::new(5); // b is on the stack, 5 is on the heap
    println!("b = {}", b);

    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
    // The List enum is recursive, so its size is unknown at compile time.
    // Box allows us to define a recursive type by providing a fixed size for the recursive part.
}
```

- **Storage**: The `Box<T>` itself is a pointer on the stack, but the data `T` it points to is stored on the heap.
- **Ownership**: `Box<T>` is a *single owner* smart pointer. When a `Box<T>` goes out of scope, both the `Box` (on the stack) and the data it points to (on the heap) are deallocated.