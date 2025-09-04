# Getting Started with Rust

## Installation

Rust is installed and managed by the `rustup` tool.

- **Linux and macOS**: Open your terminal and run:
  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```
  Follow the on-screen instructions. This will download `rustup` and install the latest stable version of Rust. You might need to configure your shell to include Cargo's `bin` directory in your `PATH` environment variable. The installer will usually prompt you to do this.

- **Windows**: Download and run `rustup-init.exe` from the official Rust website (rust-lang.org). Follow the instructions. You might need to install the C++ build tools for Visual Studio. The Rust installer will guide you through this.

- **Verification**: After installation, open a new terminal and run:
  ```bash
  rustc --version
  cargo --version
  ```
  This should display the installed versions of the Rust compiler (`rustc`) and Cargo.

## Hello, World!

Let's write your first Rust program.

1.  **Create a new directory**:
    ```bash
    mkdir hello_rust
    cd hello_rust
    ```
2.  **Create a source file**: Create a file named `main.rs` inside `hello_rust` with the following content:
    ```rust
    fn main() {
        println!("Hello, world!");
    }
    ```
    - `fn main()`: Defines the main function, the entry point of all Rust programs.
    - `println!`: A macro that prints text to the console. The `!` indicates it's a macro, not a regular function.

3.  **Compile and Run**:
    ```bash
    rustc main.rs
    ./main
    ```
    - `rustc main.rs`: Compiles the `main.rs` file, creating an executable named `main` (or `main.exe` on Windows).
    - `./main`: Executes the compiled program.

## Hello, Cargo!

Cargo is Rust's build system and package manager. It handles:
- Building your code.
- Downloading the libraries your code depends on.
- Compiling those libraries.

### Creating a Project with Cargo

Instead of manually creating files, you can use Cargo to set up a new project.

1.  **Create a new project**:
    ```bash
    cargo new hello_cargo
    cd hello_cargo
    ```
    This command creates a new directory `hello_cargo` with the following structure:
    ```
    hello_cargo/
    ├── Cargo.toml
    └── src/
        └── main.rs
    ```
    - `Cargo.toml`: The manifest file for your project. It contains metadata about your project and its dependencies.
    - `src/main.rs`: The default location for your project's main source code.

2.  **`Cargo.toml` content**:
    ```toml
    [package]
    name = "hello_cargo"
    version = "0.1.0"
    edition = "2021"

    [dependencies]
    ```
    - `[package]`: Defines a package.
    - `name`, `version`, `edition`: Project metadata.
    - `[dependencies]`: Where you list external crates (libraries) your project uses.

3.  **`src/main.rs` content**:
    ```rust
    fn main() {
        println!("Hello, world!");
    }
    ```
    Cargo generates a default "Hello, world!" program for you.

### Building and Running a Cargo Project

1.  **Build the project**:
    ```bash
    cargo build
    ```
    This command compiles your project and creates an executable in `target/debug/hello_cargo` (or `target\debug\hello_cargo.exe` on Windows).

2.  **Run the project**:
    ```bash
    cargo run
    ```
    This command compiles your project (if it hasn't been compiled or if changes were made) and then runs the executable.

3.  **Check for errors without building**:
    ```bash
    cargo check
    ```
    This command quickly checks your code for errors without producing an executable. It's faster than `cargo build` and useful for rapid iteration during development.

### Release Builds

For production, you'll want an optimized build.

```bash
cargo build --release
```
This command compiles your project with optimizations. The executable will be placed in `target/release/`. Release builds take longer to compile but run faster.
