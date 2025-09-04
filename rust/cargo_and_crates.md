# Rust Cargo and Crates.io

## Customizing Builds with Release Profiles

Cargo has *release profiles*, which are predefined and customizable profiles that allow you to control various compilation options, like optimization levels and debug information, for different environments.

- **`dev` profile**: Default for `cargo build`. Optimized for development, with debug info and no optimizations.
- **`release` profile**: Default for `cargo build --release`. Optimized for release, with optimizations and no debug info.

You can customize these in `Cargo.toml`:

```toml
[profile.dev]
enable-debug-info = true

[profile.release]
opt-level = 3
```

## Publishing Libraries on Crates.io

Crates.io is the Rust community's package registry. You can publish your library crates to share them with others.

### Before Publishing

- **Documentation Comments**: Use `///` for documentation for the item directly following it, and `//!` for documentation for the enclosing item.
  ```rust
  /// Adds two to the number given.
  ///
  /// # Examples
  ///
  /// ```
  /// let arg = 5;
  /// let answer = my_crate::add_two(arg);
  /// assert_eq!(7, answer);
  /// ```
  pub fn add_two(a: i32) -> i32 {
      a + 2
  }
  ```
- **`cargo doc`**: Generates HTML documentation from your doc comments. You can view it by opening `target/doc/<your_crate_name>/index.html` in your browser.
- **`cargo publish --dry-run`**: Checks if your crate would successfully publish without actually doing so.

### Publishing

1.  **Register an account**: Go to crates.io and log in with your GitHub account.
2.  **Get an API token**: On your crates.io account settings page, generate a new API token.
3.  **Log in Cargo**: `cargo login <your_api_token>`.
4.  **Publish**: `cargo publish`.

## Organizing Large Projects with Workspaces

A workspace is a set of packages that share the same `Cargo.lock` and output directory. It's useful for managing multiple related crates.

### Creating a Workspace

1.  Create a new directory for the workspace:
    ```bash
    mkdir my_workspace
    cd my_workspace
    ```
2.  Create a `Cargo.toml` file in the workspace root:
    ```toml
    # my_workspace/Cargo.toml
    [workspace]
    members = [
        "adder",
        "add_one",
    ]
    ```
3.  Create member crates:
    ```bash
    cargo new adder
    cargo new add_one
    ```

Now, `cargo build` or `cargo test` run for all members in the workspace.

## Installing Binaries from Crates.io with `cargo install`

`cargo install` allows you to install binary crates (executables) from crates.io.

```bash
cargo install ripgrep
```

This compiles and installs the binary to Cargo's bin directory (usually `~/.cargo/bin`), which should be in your system's `PATH`.

## Extending Cargo with Custom Commands

Cargo is designed to be extensible. Any executable in your `PATH` whose name starts with `cargo-` can be run as a Cargo subcommand.

For example, if you have an executable named `cargo-foo` in your `PATH`, you can run it with `cargo foo`.

