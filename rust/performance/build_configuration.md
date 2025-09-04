
> **NOTE:** For the impatient or forgetful, [`cargo-wizard`](https://github.com/Kobzol/cargo-wizard) encapsulates the information presented in these notes and can help you choose an appropriate build configuration.

> **NOTE:** Cargo only looks at the profile settings in the `Cargo.toml` file at the root of the workspace. Profile settings defined in dependencies are ignored. Therefore, these options are mostly relevant for binary crates, not library crates.

---
## Release Builds

The most simple but easy to overlook change that you could make is to add the `--release` flag.

> **NOTE:** Dev builds are the default. They are good for debugging, but are not optimized. They are produced if you run `cargo build` or `cargo run`.

When you use `cargo build`, the compiled code will be placed in the `target/debug/` directory. `cargo run` will run the dev build.

> **NOTE:** Release builds are much more optimized, omit debug assertions and integer overflow checks, and omit debug info. They're about 10-100x faster than dev builds.

When you use `cargo build --release`, the compiled release code will be placed in `target/release/` directory and you can use `cargo run --release` to run the release build.

---
## Maximizing Runtime Speed

The following build configuration options are designed primarily to maximize runtime speed. Some of them may also reduce binary size.

### Codegen Units

The rust compiler splits crates into multiple codegen units to parallelize (and thus speed up) compilation. However, this might cause it to miss some potential optimizations. 

You may be able to *improve runtime speed* and *reduce binary size*, at the cost of *increased compile times*, by *setting the number of units to one*. Add these lines to the `Cargo.toml` file:

```toml
[profile.release]
codegen-units = 1
```

### Link-time Optimization

**Link-time optimization (LTO)** is a whole-program optimization technique that can improve runtime speed by 10-20% or more, and also reduce binary size at the cost of worse compile times.

It comes in several forms.

```toml
[profile.release]
lto = false
```

The first form of LTO is *thin local LTO*, a lightweight form of LTO. By default the compiler uses this for any build that involves a non-zero level of optimization. This includes release builds.

```toml
[profile.release]
lto = "thin"
```

The second form of LTO is *thin LTO*, which is a little more aggressive, and likely to improve runtime speed and reduce binary size while also increasing compile times. Use `lto = "thin"` in `Cargo.toml` to enable it.

```toml
[profile.release]
lto = "fat"
```

The third form of LTO is *fat LTO*, which is even more aggressive, and may improve performance and reduce binary size further while increasing build times again. Use `lto = "fat"` in `Cargo.toml` to enable it.

```toml
[profile.release]
lto = "off"
```

Finally, it is possible to fully disable LTO, which will likely worsen runtime speed and increase binary size but reduce compile times. Use `lto = "off"` in `Cargo.toml` for this.

### Alternative Allocators

It is possible to replace the default (system) heap allocator used by a Rust program with an alternative allocator.

> **NOTE:** The exact effect will depend on the individual program and the alternative allocator chosen, but large improvements in runtime speed and large reductions in memory usage have been seen in practice.

> **NOTE:** The effect will also vary across platforms, because each platform’s system allocator has its own strengths and weaknesses.

> **NOTE:** The use of an alternative allocator is also likely to increase binary size and compile times.

#### jemalloc

One popular alternative allocator for Linux or Mac is jemalloc, usable via the `tikv-jemallocator` create. To use it, add a dependency to your `Cargo.toml` file:

```toml
[dependencies]
tikv-jemallocator = "0.5"
```

Then add the following to your Rust code, e.g. at the top of `src/main.rs`:

```rust
#[global_allocator]
static GLOBAL: tikv_jemallocator::Jemalloc = tikv_jemallocator::Jemalloc;
```

> **NOTE:** The system running the compiled program has to be configured to support THP. See [this blog post](https://kobzol.github.io/rust/rustc/2023/10/21/make-rust-compiler-5percent-faster.html) for more details.

##### Linux

On Linux, jemalloc can be configured to use [transparent huge pages](https://www.kernel.org/doc/html/next/admin-guide/mm/transhuge.html) (THP). This can further speed up programs, possibly at the cost of higher memory usage.

Do this by setting the `MALLOC_CONF` environment variable (or perhaps [`_RJEM_MALLOC_CONF`](https://github.com/tikv/jemallocator/issues/65)) appropriately before building your program, for example:

```bash
MALLOC_CONF="thp:always,metadata_thp:always" cargo build --release
```

#### mimalloc

Another alternative allocator that works on many platforms is `mimalloc`, usable via the `mimalloc` crate. To use it, add a dependency to your `Cargo.toml` file:

```toml
[dependencies]
mimalloc = "0.1"
```

Then add the following to your Rust code, e.g. at the top of `src/main.rs`:

```rust
#[global_allocator]
static GLOBAL: mimalloc::MiMalloc = mimalloc::MiMalloc;
```

### CPU Specific Instructions

You can tell the compiler to generate the newest (and potentially fastest) instructions specific to a certain CPU architecture.

To request these instructions from the command line, use the `-C target-cpu=native` flag. For example:

```bash
RUSTFLAGS="-C target-cpu=native" cargo build --release
```

Alternatively, to request these instructions from a [`config.toml`](https://doc.rust-lang.org/cargo/reference/config.html) file (for one or more projects), add these lines:

```toml
[build]
rustflags = ["-C", "target-cpu=native"]
```

> **NOTE:** This can improve runtime speed, especially if the compiler finds vectorization opportunities in your code.

---
## Minimizing Binary Size

The following build configuration options are designed primarily to minimize binary size. Their effects on runtime speed vary.

#### Optimization Level

You can request an optimization level that aims to minimize the binary size by adding these lines to the `Cargo.toml` file:

```toml
[profile.release]
opt-level = "z"
```

> **NOTE:** This may also reduce runtime speed.

An alternative is `opt-level = "s"`, which targets minimal binary size a little less aggressively. Compared to `opt-level = "z"`, it allows [slightly more inlining](https://doc.rust-lang.org/rustc/codegen-options/index.html#inline-threshold) and also the vectorization of loops.

### Abort on `panic!`

If you do not need to unwind on panic, e.g. because your program doesn’t use [`catch_unwind`](https://doc.rust-lang.org/std/panic/fn.catch_unwind.html), you can tell the compiler to simply [abort on panic](https://doc.rust-lang.org/cargo/reference/profiles.html#panic). On panic, your program will still produce a backtrace.

This might reduce binary size and increase runtime speed slightly, and may even reduce compile times slightly. Add these lines to the `Cargo.toml` file:

```toml
[profile.release]
panic = "abort"
```

### Strip Debug Info and Symbols

You can tell the compiler to [strip](https://doc.rust-lang.org/cargo/reference/profiles.html#strip) debug info and symbols from the compiled binary. Add these lines to `Cargo.toml` to strip just debug info:

```toml
[profile.release]
strip = "debuginfo"
```

Alternatively, use `strip = "symbols"` to strip both debug info and symbols.

> **NOTE:** Prior to Rust 1.77, the default behaviour was to do no stripping. [As of Rust 1.77](https://blog.rust-lang.org/2024/03/21/Rust-1.77.0.html#enable-strip-in-release-profiles-by-default) the default behaviour is to strip debug info in release builds.

Stripping debug info can greatly reduce binary size. On Linux, the binary size of a small Rust programs might shrink by 4x when debug info is stripped. Stripping symbols can also reduce binary size, though generally not by as much. [**Example**](https://github.com/nnethercote/counts/commit/53cab44cd09ff1aa80de70a6dbe1893ff8a41142). The exact effects are platform-dependent.

> **NOTE:** Stripping makes your compiled program more difficult to debug and profile.

### Other Ideas

For more advanced binary size minimization techniques, consult the comprehensive documentation in the excellent [`min-sized-rust`](https://github.com/johnthagen/min-sized-rust) repository.

---
## Minimizing Compile Times

The following build configuration options are designed primarily to minimize compile times.

### Linking

A big part of compile time is actually linking time, particularly when rebuilding a program after a small change. It is possible to select a faster linker than the default one.

One option is [lld](https://lld.llvm.org/), which is available on Linux and Windows. To specify lld from the command line, use the `-C link-arg=-fuse-ld=lld` flag. For example:

```bash
RUSTFLAGS="-C link-arg=-fuse-ld=lld" cargo build --release
```

Alternatively, to specify lld from a [`config.toml`](https://doc.rust-lang.org/cargo/reference/config.html) file (for one or more projects), add these lines:

```toml
[build]
rustflags = ["-C", "link-arg=-fuse-ld=lld"]
```

> **NOTE:** lld is not fully supported for use with Rust, but it should work for most use cases on Linux and Windows. There is a [GitHub Issue](https://github.com/rust-lang/rust/issues/39915#issuecomment-618726211) tracking full support for lld.

Another option is [mold](https://github.com/rui314/mold), which is currently available on Linux. Simply substitute `mold` for `lld` in the instructions above. mold is often faster than lld. [**Example**](https://davidlattimore.github.io/posts/2024/02/04/speeding-up-the-rust-edit-build-run-cycle.html). It is also much newer and may not work in all cases.

> **NOTE:** Unlike the other options in this chapter, there are no trade-offs here! Alternative linkers can be dramatically faster, without any downsides.

### Disable Debug Info Generation

Although release builds give the best performance, many people use dev builds while developing because they build more quickly. 

If you use dev builds but don't use the debugger, consider disabling debuginfo. This can improve dev build times significantly, by as much as 20-40%.

To disable debug info generation, add these lines to the `Cargo.toml` file:

```toml
[profile.dev]
debug = false
```

> **NOTE:** This means that stack traces will not contain line information. If you want to keep that line information, but do not require full information for the debugger, you can use `debug = "line-tables-only"` instead, which still gives most of the compile time benefits.

### Experimental Parallel Front-end

If you use nightly Rust, you can enable the *experimental parallel front-end*. It may reduce compile times at the cost of higher compile-time memory usage. It won't affect the quality of the generated code.

You can do that by adding `-Zthreads=N` to `RUSTFLAGS`, for example:

```bash
RUSTFLAGS="-Zthreads=8" cargo build --release
```

Alternatively, to enable the parallel front-end from a [`config.toml`](https://doc.rust-lang.org/cargo/reference/config.html) file (for one or more projects), add these lines:

```toml
[build]
rustflags = ["-Z", "threads=8"]
```

> **NOTE:** Values other than `8` are possible, but that is the number that tends to give the best results.

In the best cases, the experimental parallel front-end *reduces compile times by up to 50%*. But the effects vary widely and depend on the characteristics of the code and its build configuration, and *for some programs there is no compile time improvement*.

### Cranelift Codegen Back-end

If you use *nightly Rust* you can enable the *Cranelift codegen back-end* on some platforms. It may reduce compile times at the cost of lower quality generated code, and therefore is recommended for dev builds rather than release builds.

First, install the back-end with this `rustup` command:

```bash
rustup component add rustc-codegen-cranelift-preview --toolchain nightly
```

To select Cranelift from the command line, use the `-Zcodegen-backend=cranelift` flag. For example:

```bash
RUSTFLAGS="-Zcodegen-backend=cranelift" cargo +nightly build
```

Alternatively, to specify Cranelift from a [`config.toml`](https://doc.rust-lang.org/cargo/reference/config.html) file (for one or more projects), add these lines:

```toml
[unstable]
codegen-backend = true

[profile.dev]
codegen-backend = "cranelift"
```

For more information, see the [Cranelift documentation](https://github.com/rust-lang/rustc_codegen_cranelift).

---
## Custom Profiles

In addition to the `dev` and `release` profiles, Cargo supports [custom profiles](https://doc.rust-lang.org/cargo/reference/profiles.html#custom-profiles). It might be useful, for example, to create a custom profile halfway between `dev` and `release` if you find the runtime speed of dev builds insufficient and the compile times of release builds too slow for everyday development.

---