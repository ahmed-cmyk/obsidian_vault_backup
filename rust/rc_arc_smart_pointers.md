# Rust Rc and Arc Smart Pointers

## `Rc<T>`: Reference Counted Smart Pointer

`Rc<T>` (reference counting) allows for *multiple ownership* of data. It keeps track of the number of references to a value, and when the count drops to zero, the value is cleaned up.

- **Use Case**: When you want to share data among multiple parts of your program, and you don't know at compile time which part will be the last to use the data.
- **Storage**: The data itself is on the heap. The `Rc<T>` pointer and the reference count are also managed on the heap.
- **Immutability**: `Rc<T>` only allows immutable references to the data it owns. To get mutability with `Rc<T>`, you need to combine it with `RefCell<T>`.

```rust
enum ListRc {
    Cons(i32, Rc<ListRc>),
    Nil,
}

use ListRc::{Cons, Nil};
use std::rc::Rc;

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    println!("count after creating a = {}", Rc::strong_count(&a));
    let b = Cons(3, Rc::clone(&a)); // Rc::clone increments the reference count
    println!("count after creating b = {}", Rc::strong_count(&a));
    {
        let c = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a));
    }
    println!("count after c goes out of scope = {}", Rc::strong_count(&a));
}
```

## `Arc<T>`: Atomic Reference Counted Smart Pointer

`Arc<T>` (atomic reference counting) is similar to `Rc<T>` but is safe to use across multiple threads. It provides thread-safe multiple ownership.

- **Use Case**: When you need to share data between multiple threads and `Rc<T>` is not sufficient because it's not thread-safe.
- **Performance**: `Arc<T>` has a performance overhead compared to `Rc<T>` due to atomic operations.

```rust
use std::sync::Arc;
use std::thread;

fn main() {
    let data = Arc::new(vec![1, 2, 3]);
    let mut handles = vec![];

    for i in 0..3 {
        let data_clone = Arc::clone(&data);
        let handle = thread::spawn(move || {
            println!("thread {} data: {:?}", i, *data_clone);
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```