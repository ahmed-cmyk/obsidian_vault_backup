`rustc` is the compiler for the Rust programming language, provided by the project itself. Compilers take your source code and produce binary code, either as a library or executable.

> **NOTE:** Most Rust programmers don't invoke `rustc` directly, but instead do it through [Cargo](https://doc.rust-lang.org/cargo/index.html).  

---
## Basic usage

```rust
// hello.rs
fn main() {
    println!("Hello, world!");
}
```

```bash
rustc hello.rs
```

> **NOTE:**  We only ever pass `rustc` the _crate root_, not every file we wish to compile.

> **NOTE:** This is different than how you would use a C compiler, where you invoke the compiler on each file, and then link everything together. In other words, the _crate_ is a translation unit, not a particular module.

---