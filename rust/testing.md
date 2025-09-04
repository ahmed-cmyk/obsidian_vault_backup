# Rust Testing

## How to Write Tests

Tests are Rust functions annotated with `#[test]`. They typically involve three parts:
1.  **Setup**: Get any data or state needed.
2.  **Run**: Run the code you want to test.
3.  **Assert**: Assert that the result is what you expect.

```rust
#[cfg(test)] // Only compile this module when running tests
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }

    #[test]
    fn another_test() {
        panic!("Make this test fail");
    }
}
```

## Running Tests

To run all tests in your project, use the `cargo test` command.

```bash
cargo test
```

- **Output**: Cargo compiles your test code and then runs the generated test binary. It reports the number of tests passed, failed, and ignored.

### Controlling How Tests Are Run

- **Running a single test**: Specify the test function's name.
  ```bash
  cargo test another_test
  ```

- **Running multiple tests by name**: Specify part of the test function's name.
  ```bash
  cargo test it
  ```

- **Ignoring tests**: Use `#[ignore]` attribute.
  ```rust
  #[test]
  #[ignore]
  fn expensive_test() {
      // code that takes a long time to run
  }
  ```
  To run ignored tests: `cargo test -- --ignored`.

- **Running tests in parallel**: By default, tests run in parallel. To run sequentially: `cargo test -- --test-threads=1`.

## Assertion Macros

Rust provides several macros for making assertions in tests:

- **`assert!(condition)`**: Panics if `condition` is `false`.
- **`assert_eq!(left, right)`**: Panics if `left` and `right` are not equal. Uses `PartialEq` and `Debug` traits.
- **`assert_ne!(left, right)`**: Panics if `left` and `right` are equal.

```rust
#[test]
fn greeting_contains_name() {
    let result = greeting("Carol");
    assert!(result.contains("Carol"));
}

fn greeting(name: &str) -> String {
    format!("Hello, {}", name)
}

#[test]
fn add_two_test() {
    assert_eq!(4, add_two(2));
}

fn add_two(a: i32) -> i32 {
    a + 2
}
```

## Checking for Panics with `#[should_panic]`

Use `#[should_panic]` to test that a function panics under certain conditions.

```rust
#[test]
#[should_panic]
fn greater_than_100() {
    Guess::new(200);
}

// Optionally, check for a specific panic message
#[test]
#[should_panic(expected = "Guess value must be less than or equal to 100")]
fn greater_than_100_message() {
    Guess::new(200);
}

pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }
        Guess { value }
    }
}
```

## Test Organization

Rust tests are typically organized into two categories:

- **Unit Tests**: Test individual units of code in isolation. They are placed in the same file as the code they're testing, within a `#[cfg(test)]` module.

- **Integration Tests**: Test how multiple parts of your library work together. They are placed in a separate `tests` directory at the root of your project.

### Integration Tests Example

1.  Create a `tests` directory at the root of your project (next to `src`).
2.  Create a new file inside `tests`, e.g., `tests/integration_test.rs`.
3.  In `integration_test.rs`, you can bring your library into scope:
    ```rust
    // tests/integration_test.rs
    use your_crate_name; // Replace your_crate_name with your actual crate name

    #[test]
    fn it_adds_two() {
        assert_eq!(4, your_crate_name::add_two(2));
    }
    ```

Integration tests are run with `cargo test` just like unit tests. They are compiled as separate crates.

