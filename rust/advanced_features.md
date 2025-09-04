# Rust Advanced Features

## Unsafe Rust

Unsafe Rust allows you to opt out of some of Rust's compile-time guarantees, such as memory safety. When you use `unsafe` code, you take responsibility for upholding those guarantees manually. The `unsafe` keyword doesn't turn off the borrow checker or disable other Rust safety features; it just gives you five additional superpowers:

1.  **Dereference a raw pointer**
2.  **Call an `unsafe` function or method**
3.  **Access or modify a mutable static variable**
4.  **Implement an `unsafe` trait**
5.  **Access fields of `union`s**

```rust
fn main() {
    let mut num = 5;

    let r1 = &num as *const i32; // Immutable raw pointer
    let r2 = &mut num as *mut i32; // Mutable raw pointer

    unsafe {
        println!("r1 is: {}", *r1);
        println!("r2 is: {}", *r2);
    }

    // Calling an unsafe function
    unsafe fn dangerous() {}
    unsafe { dangerous(); }
}
```

- **Raw Pointers**: `*const T` (immutable) and `*mut T` (mutable). They don't have lifetimes and can be null. They don't guarantee valid memory or type safety.
- **When to use `unsafe`**: Interfacing with C/C++ code (FFI), implementing operating system calls, or building performance-critical data structures where Rust's safe abstractions introduce unacceptable overhead.

## Advanced Traits

### Associated Types

Associated types specify a placeholder type in a trait definition. This allows a trait to define a type that will be determined by the implementing type.

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}

impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        // ...
        Some(1)
    }
}
```

### Default Generic Type Parameters and Operator Overloading

Allows you to specify a default concrete type for a generic type parameter. This is often used in operator overloading.

```rust
use std::ops::Add;

#[derive(Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Point;

    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 1, y: 0 };
    let p2 = Point { x: 2, y: 3 };
    println!("{:?}", p1 + p2);
}
```

### Fully Qualified Syntax for Disambiguation

When a method has the same name as another method, you might need to use fully qualified syntax to specify which method you mean.

```rust
trait Pilot {
    fn fly(&self);
}

trait Wizard {
    fn fly(&self);
}

struct Human;

impl Pilot for Human {
    fn fly(&self) {
        println!("This is your captain speaking.");
    }
}

impl Wizard for Human {
    fn fly(&self) {
        println!("Up!");
    }
}

impl Human {
    fn fly(&self) {
        println!("*waving arms furiously*");
    }
}

fn main() {
    let person = Human;
    person.fly(); // Calls Human's fly method
    Pilot::fly(&person); // Calls Pilot's fly method
    Wizard::fly(&person); // Calls Wizard's fly method
}
```

### Supertraits

Traits can depend on other traits. This is useful when you want to require that a type implements another trait before it can implement your trait.

```rust
use std::fmt::Display;

trait OutlinePrint: Display {
    fn outline_print(&self) {
        let output = self.to_string();
        let len = output.len();
        println!("{}", "-".repeat(len + 4));
        println!("| {} |", output);
        println!("{}", "-".repeat(len + 4));
    }
}

struct PointOutline {
    x: i32,
    y: i32,
}

impl Display for PointOutline {
    fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

impl OutlinePrint for PointOutline {}

fn main() {
    let p = PointOutline { x: 1, y: 2 };
    p.outline_print();
}
```

### The Newtype Pattern

The newtype pattern is a way to create a new type from an existing type to provide type safety and abstraction.

```rust
struct Millimeters(u32);
struct Meters(u32);

// You can implement traits for your newtype
// impl Add for Millimeters { ... }
```

## Advanced Types

### Type Aliases

Type aliases (`type`) create a new name for an existing type. They don't create a new type, just a synonym.

```rust
type Kilometers = i32;

fn main() {
    let x: Kilometers = 5;
    let y: i32 = 10;
    println!("x + y = {}", x + y);
}
```

### The "Never" Type (`!`) 

The `!` type is called the "never" type because it represents the type that never returns. Functions that panic or loop forever have the `!` return type.

```rust
fn bar() -> ! {
    panic!("This function never returns!");
}
```

### Dynamically Sized Types and the `Sized` Trait

Dynamically sized types (DSTs) are types whose size cannot be known at compile time (e.g., `str`, `[T]`). The `Sized` trait indicates that a type's size is known at compile time. By default, all generic type parameters have a `Sized` trait bound.

```rust
// fn generic<T>(t: T) { // T: Sized is implicitly added
// fn generic<T: ?Sized>(t: &T) { // ?Sized removes the Sized trait bound
```

## Advanced Functions and Closures

### Function Pointers

Function pointers allow you to pass functions as arguments to other functions.

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);
    println!("The answer is: {}", answer);
}
```

### Returning Closures

Returning closures directly from functions can be tricky because closures are represented by traits. You often need to use `Box<dyn Fn...>`.

```rust
// fn returns_closure() -> Fn(i32) -> i32 { // This won't compile
//     |x| x + 1
// }

fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn main() {
    let f = returns_closure();
    println!("Returned closure result: {}", f(5));
}
```

## Macros

Macros are a way to write code that writes other code (metaprogramming). They expand at compile time, before the code is compiled.

### Declarative Macros (`macro_rules!`) 

These are the most common type of macros, defined using `macro_rules!`.

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $( temp_vec.push($x); )*
            temp_vec
        }
    };
}

fn main() {
    let v = vec![1, 2, 3];
    println!("{:?}", v);
}
```

### Procedural Macros

More powerful and flexible, they operate on the AST (Abstract Syntax Tree) of the Rust code. There are three types:

-   **Function-like macros**: `#[proc_macro]`
-   **Derive macros**: `#[proc_macro_derive]` (e.g., `#[derive(Debug)]`)
-   **Attribute macros**: `#[proc_macro_attribute]`

