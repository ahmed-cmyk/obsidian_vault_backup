# Rust Modules and Crates

## Packages and Crates

- **Packages**: A Cargo feature that lets you build, test, and share crates. A package contains a `Cargo.toml` file and a `src` folder. A package can contain:
    - Zero or one library crate.
    - Zero or more binary crates.
- **Crates**: The smallest amount of code that the Rust compiler considers at a time. Crates can be:
    - **Binary crates**: Executables (e.g., `src/main.rs`).
    - **Library crates**: Produce a library that other projects can use (e.g., `src/lib.rs`).

When you run `cargo new <project_name>`, Cargo creates a package containing a binary crate with `src/main.rs`.

## Defining Modules to Control Scope and Privacy

Modules group together related code within a crate. They control the visibility (public or private) of items.

- **`mod` keyword**: Used to declare a module.
- **Privacy**: By default, all items (functions, methods, structs, enums, modules, constants) are private within their containing module. To make an item public, use the `pub` keyword.

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
}
```

## Paths for Referring to an Item in the Module Tree

To refer to an item, you use its *path*. Paths can be:

- **Absolute path**: Starts from a crate root (e.g., `crate::front_of_house::hosting::add_to_waitlist`).
- **Relative path**: Starts from the current module (e.g., `hosting::add_to_waitlist`).

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```

## Bringing Paths into Scope with the `use` Keyword

The `use` keyword creates a shortcut to a path, making it easier to refer to items without typing the full path every time.

```rust
use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

- **Idiomatic `use` paths**: For functions, `use` the parent module. For structs, enums, and other items, `use` the item itself.

```rust
// Idiomatic for functions
use std::collections::HashMap;

// Not idiomatic for functions (unless there's a naming conflict)
// use std::collections::HashMap::new;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```

- **`as` keyword**: To bring two items with the same name into scope.
  ```rust
  use std::fmt::Result;
  use std::io::Result as IoResult;
  ```

- **`pub use`**: Re-exporting a name, making it available to external code that uses your crate.
  ```rust
  pub use crate::front_of_house::hosting;
  ```

## Separating Modules into Different Files

As modules grow, you can move their definitions into separate files.

- **`mod <name>;`**: In `src/lib.rs` or `src/main.rs`, declare a module with `mod <name>;`. Rust will look for the module's code in:
    - `src/<name>.rs`
    - `src/<name>/mod.rs`

Example:

`src/lib.rs`:
```rust
mod front_of_house;
```

`src/front_of_house.rs`:
```rust
pub mod hosting {
    pub fn add_to_waitlist() {}
}
```

For nested modules, you can create subdirectories:

`src/front_of_house/mod.rs`:
```rust
pub mod hosting;
```

`src/front_of_house/hosting.rs`:
```rust
pub fn add_to_waitlist() {}
```

