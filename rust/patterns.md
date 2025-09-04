# Rust Patterns and Matching

## All the Places Patterns Can Be Used

Patterns can be used in several places in Rust code:

-   **`match` expressions**: The most common use case, where a value is matched against a series of patterns.
-   **`if let` expressions**: For concisely handling a single pattern.
-   **`while let` loops**: To loop while a pattern continues to match.
-   **`for` loops**: To destructure values from an iterator.
-   **`let` statements**: To destructure values.
-   **Function parameters**: To destructure values passed into a function.

## Refutability: Whether a Pattern Might Fail to Match

Patterns can be either *refutable* or *irrefutable*:

-   **Irrefutable patterns**: Patterns that will always match for any possible value. Used in `let` statements and `for` loops because they must match every time.
    ```rust
    let x = 5; // `x` is an irrefutable pattern
    let (x, y) = (1, 2); // `(x, y)` is an irrefutable pattern
    ```

-   **Refutable patterns**: Patterns that might fail to match for some possible value. Used in `match` arms, `if let`, and `while let` because these constructs are designed to handle possible failure to match.
    ```rust
    let some_option = Some(5);
    if let Some(x) = some_option { // `Some(x)` is a refutable pattern
        println!("{}", x);
    }
    ```

## Pattern Syntax

Rust provides a rich set of pattern syntax:

-   **Literals**: Match against exact values.
    ```rust
    let x = 1;
    match x {
        1 => println!("one"),
        2 => println!("two"),
        _ => println!("anything"),
    }
    ```

-   **Named Variables**: Bind the matched value to a variable.
    ```rust
    let x = Some(5);
    let y = 10;

    match x {
        Some(50) => println!("Got 50"),
        Some(y) => println!("Matched, y = {:?}", y), // y here shadows the outer y
        _ => println!("Default case, x = {:?}", x),
    }
    ```

-   **Multiple Patterns (`|`)**: Match against multiple possible patterns.
    ```rust
    let x = 1;
    match x {
        1 | 2 => println!("one or two"),
        _ => println!("anything"),
    }
    ```

-   **Ranges of Values (`..=`)**: Match against a range of numeric or char values.
    ```rust
    let x = 5;
    match x {
        1..=5 => println!("one through five"),
        _ => println!("something else"),
    }
    ```

-   **Destructuring Structs**: Extract values from struct fields.
    ```rust
    struct Point {
        x: i32,
        y: i32,
    }

    let p = Point { x: 0, y: 7 };

    match p {
        Point { x: 0, y: y_val } => println!("On the y axis at {}", y_val),
        Point { x, y } => println!("On neither axis: ({}, {})", x, y),
    }
    ```

-   **Destructuring Enums**: Extract values from enum variants.
    ```rust
    enum Message {
        Quit,
        Move { x: i32, y: i32 },
        Write(String),
        ChangeColor(i32, i32, i32),
    }

    let msg = Message::ChangeColor(0, 160, 255);

    match msg {
        Message::Quit => println!("The Quit variant has no data to destructure."),
        Message::Move { x, y } => {
            println!("Move in the x direction {} and in the y direction {}", x, y);
        }
        Message::Write(text) => println!("Text message: {}", text),
        Message::ChangeColor(r, g, b) => {
            println!("Change the color to red {}, green {}, and blue {}", r, g, b);
        }
    }
    ```

-   **Destructuring Tuples**: Extract values from tuple elements.
    ```rust
    let point = (3, 5);
    match point {
        (x, y) => println!("({}, {})", x, y),
    }
    ```

-   **`_` (Wildcard)**: Ignores a value.
    ```rust
    fn foo(_: i32, y: i32) {
        println!("This code only uses the y parameter: {}", y);
    }
    ```

-   **`..` (Range/Ignore Remaining)**: Ignores remaining parts of a value.
    ```rust
    struct Point3D {
        x: i32,
        y: i32,
        z: i32,
    }

    let p = Point3D { x: 0, y: 7, z: 0 };

    match p {
        Point3D { x, .. } => println!("x is {}", x),
    }

    let numbers = (2, 4, 8, 16, 32);
    match numbers {
        (first, .., last) => {
            println!("Some numbers: {}, {}", first, last);
        }
    }
    ```

-   **`@` Bindings**: Binds a value to a variable while also matching against it.
    ```rust
enum MessageWithId {
    Hello { id: i32 },
}

fn main() {
    let msg = MessageWithId::Hello { id: 5 };

    match msg {
        MessageWithId::Hello { id: id_variable @ 3..=7 } => {
            println!("Found an ID in range: {}", id_variable)
        },
        MessageWithId::Hello { id: 10..=12 } => {
            println!("Found an ID in another range")
        },
        MessageWithId::Hello { id } => {
            println!("Found some other ID: {}", id)
        },
    }
}
    ```

