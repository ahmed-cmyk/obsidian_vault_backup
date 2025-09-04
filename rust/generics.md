# Rust Generics

Generics are abstract stand-ins for concrete types or other properties. They help reduce code duplication by allowing functions, structs, and enums to work with various data types without needing to write separate implementations for each.

### In Functions

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }
    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];
    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];
    let result = largest(&char_list);
    println!("The largest char is {}", result);
}
```

- `T`: A generic type parameter.
- `PartialOrd + Copy`: These are *trait bounds*, meaning `T` must implement the `PartialOrd` trait (for comparison) and the `Copy` trait (because we're copying values). If `T` doesn't implement `Copy`, we'd need to use references or `Clone`.

### In Structs

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer_point = Point { x: 5, y: 10 };
    let float_point = Point { x: 1.0, y: 4.0 };
    // let mixed_point = Point { x: 5, y: 4.0 }; // This would cause a compile-time error
}
```

### In Enums

```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```