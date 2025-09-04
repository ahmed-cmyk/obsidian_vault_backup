# Rust Error Handling

## Unrecoverable Errors with `panic!`

When a program encounters an unrecoverable error, Rust will `panic!`. This means the program will exit, and it will print a message indicating where the panic occurred, along with a backtrace.

- **When to `panic!`**: Typically used for bugs, situations where a program reaches a state it cannot possibly recover from, or when a contract is violated (e.g., array index out of bounds).

```rust
fn main() {
    // Example of a panic due to out-of-bounds access
    // let v = vec![1, 2, 3];
    // v[99]; // This will panic!

    // Explicit panic!
    // panic!("Crash and burn!");
}
```

- **Backtrace**: Provides information about the call stack, helping to pinpoint the source of the panic. You can enable it by setting the `RUST_BACKTRACE` environment variable to `1`.

## Recoverable Errors with `Result<T, E>`

For recoverable errors, Rust uses the `Result<T, E>` enum. This enum is defined as:

```rust
enum Result<T, E> {
    Ok(T), // Represents success and contains a value of type T
    Err(E), // Represents an error and contains a value of type E
}
```

`Result` forces you to consider and handle potential errors, making your code more robust.

### Example: Opening a File

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => panic!("Problem opening the file: {:?}", other_error),
        },
    };
}
```

### Shortcuts for `panic!` on Error: `unwrap` and `expect`

- **`unwrap()`**: A shorthand method on `Result` that will return the value inside an `Ok` variant. If the `Result` is an `Err` variant, `unwrap` will call `panic!`.
  ```rust
  // let greeting_file = File::open("hello.txt").unwrap(); // Panics if file not found
  ```

- **`expect()`**: Similar to `unwrap()`, but allows you to provide a custom panic message.
  ```rust
  // let greeting_file = File::open("hello.txt")
  //     .expect("hello.txt should be included in this project");
  ```

### Propagating Errors: The `?` Operator

The `?` operator is a concise way to propagate errors up the call stack. It can only be used in functions that return a `Result` or `Option`.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}

// A more concise version using chaining
fn read_username_from_file_concise() -> Result<String, io::Error> {
    let mut username = String::new();
    File::open("hello.txt")?.read_to_string(&mut username)?;
    Ok(username)
}

// Even more concise using fs::read_to_string
use std::fs;
fn read_username_from_file_shortest() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

When `?` is applied to a `Result`:
- If the `Result` is `Ok`, the value inside the `Ok` is returned from the expression, and the program continues.
- If the `Result` is `Err`, the `Err` is returned from the *current function*, effectively propagating the error.

## To `panic!` or Not to `panic!`

Deciding whether to `panic!` or return a `Result` depends on the context:

- **`panic!`**: Use when a bug is detected, and the program is in an unrecoverable state. This indicates a programming error that should be fixed.
- **`Result`**: Use for operations where failure is a reasonable possibility and you want to give the calling code a chance to handle the error gracefully.

Consider the context of your code: if a function is part of a public API, it's generally better to return a `Result` to allow users of your library to handle errors. If it's an internal helper function where a failure indicates a bug in your own code, `panic!` might be acceptable.

