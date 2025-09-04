# Rust Iterators and Closures

## Closures: Anonymous Functions that Capture Their Environment

Closures are anonymous functions that you can save in a variable or pass as arguments to other functions. Unlike regular functions, closures can capture values from the scope in which they are defined.

### Defining and Calling Closures

```rust
fn main() {
    let expensive_closure = |num: u32| -> u32 {
        println!("calculating slowly...");
        std::thread::sleep(std::time::Duration::from_secs(2));
        num
    };

    let intensity = 10;
    let random_number = 7;

    if intensity < 25 {
        println!("Today, do {} pushups!", expensive_closure(intensity));
        println!("Next, do {} situps!", expensive_closure(random_number));
    } else {
        println!("Take a break today! Remember to stay hydrated!");
    }
}
```

- **Syntax**: `|param1, param2| { expression }`.
- **Type Inference**: Rust can often infer the types of parameters and return values for closures.

### Capturing the Environment

Closures can capture values from their enclosing scope in three ways, corresponding to the three `Fn` traits:

- **`FnOnce`**: Consumes the variables it captures from its enclosing scope. It can be called at most once.
- **`FnMut`**: Can change the environment because it mutably borrows values.
- **`Fn`**: Borrows values immutably from the environment.

Rust infers which `Fn` trait to use based on how the closure interacts with its environment.

```rust
fn main() {
    let x = vec![1, 2, 3];

    let equal_to_x = move |z| z == x; // `move` forces the closure to take ownership of `x`

    // println!("can't use x here: {:?}", x); // This would cause a compile-time error

    let y = vec![1, 2, 3];
    assert!(equal_to_x(y));
}
```

## Iterators: Processing a Sequence of Items

Iterators provide a way to process a sequence of items. They are *lazy*, meaning they don't do any work until you call a *consuming adapter*.

### The `Iterator` Trait

All iterators implement the `Iterator` trait, which requires a `next` method:

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    // ... other methods with default implementations
}
```

### Creating Iterators

- **`iter()`**: Creates an iterator that yields immutable references.
- **`into_iter()`**: Creates an iterator that yields owned values.
- **`iter_mut()`**: Creates an iterator that yields mutable references.

```rust
fn main() {
    let v1 = vec![1, 2, 3];

    let v1_iter = v1.iter();

    for val in v1_iter {
        println!("Got: {}", val);
    }

    // Using next() directly
    let mut v1_iter = v1.iter();
    assert_eq!(v1_iter.next(), Some(&1));
    assert_eq!(v1_iter.next(), Some(&2));
    assert_eq!(v1_iter.next(), Some(&3));
    assert_eq!(v1_iter.next(), None);
}
```

### Iterator Adapters

Methods that consume an iterator and produce another iterator. They are also lazy.

- **`map`**: Transforms each item in an iterator.
- **`filter`**: Keeps only items that satisfy a predicate.

```rust
fn main() {
    let v1: Vec<i32> = vec![1, 2, 3];

    let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

    assert_eq!(v2, vec![2, 3, 4]);

    let shoes = vec![
        Shoe { size: 10, style: String::from("sneaker") },
        Shoe { size: 13, style: String::from("sandal") },
        Shoe { size: 10, style: String::from("boot") },
    ];

    let in_my_size = shoes_in_size(shoes, 10);

    for s in in_my_size {
        println!("Found shoe: {:?}", s);
    }
}

#[derive(PartialEq, Debug)]
struct Shoe {
    size: u32,
    style: String,
}

fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}
```

### Consuming Adapters

Methods that consume the iterator and produce a final value.

- **`sum`**: Sums the elements.
- **`collect`**: Gathers the elements into a collection.

```rust
fn main() {
    let v1 = vec![1, 2, 3];
    let total: i32 = v1.iter().sum();
    assert_eq!(total, 6);
}
```

## Performance of Iterators

Rust's iterators are a *zero-cost abstraction*. This means that using iterators provides the same performance as writing the equivalent code manually with loops. The compiler optimizes away the abstraction overhead.

