
[Clippy](https://github.com/rust-lang/rust-clippy) is a collection of lints to catch common mistakes in Rust code. It is an excellent tool to run on Rust code in general. It can also help with performance, because a number of the lints relate to code patterns that can cause sub-optimal performance.

---
## Basics

Once installed, it is easy to run:

```bash
cargo clippy
```

The full list of performance lints can be seen by visiting the [lint list](https://rust-lang.github.io/rust-clippy/master/) and deselecting all the lint groups except for “Perf”.

As well as making the code faster, the performance lint suggestions usually result in code that is simpler and more idiomatic, so they are worth following even for code that is not executed frequently.

Conversely, some non-performance lint suggestions can improve performance. For example, the [`ptr_arg`](https://rust-lang.github.io/rust-clippy/master/index.html#ptr_arg) style lint suggests changing various container arguments to slices, such as changing `&mut Vec<T>` arguments to `&mut [T]`. The primary motivation here is that a slice gives a more flexible API, but it may also result in faster code due to less indirection and better optimization opportunities for the compiler. [**Example**](https://github.com/fschutt/fastblur/pull/3/files).

---
## Avoiding Certain Std Types
  
- Sometimes faster alternatives exist to std types.  
- Risk: accidentally mixing in std types.  
- **Solution**: use Clippy’s `disallowed_types` lint.  

Example (`clippy.toml`):  

```toml
disallowed-types = ["std::collections::HashMap", "std::collections::HashSet"]
```

---