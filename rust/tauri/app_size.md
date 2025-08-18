
While Tauri by default provides very small binaries it doesn’t hurt to push the limits a bit, so here are some tips and tricks for reaching optimal results.

One of the simplest frontend agnostic size improvements you can do to your project is adding a Cargo profile to it.

Dependent on whether you use the stable or nightly Rust toolchain the options available to you differ a bit. It’s recommended you stick to the stable toolchain unless you’re an advanced user.

```toml
# src-tauri/Cargo.toml
[profile.dev]
incremental = true # Compile your binary in smaller steps.

[profile.release]
codegen-units = 1 # Allows LLVM to perform better optimization.
lto = true # Enables link-time-optimizations.
opt-level = "s" # Prioritizes small binary size. Use `3` if you prefer speed.
panic = "abort" # Higher performance by disabling panic handlers.
strip = true # Ensures debug symbols are removed.
```

---
