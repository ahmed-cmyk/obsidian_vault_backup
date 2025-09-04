
## **1. Logging Overhead**

- Logging can be expensive if you do it inside a **hot path** (code that runs often).    
- Even if logging is “disabled,” the **data preparation** (string formatting, cloning, allocation) may still happen unless you guard against it.

**Bad (wasteful):**

```rust
log::debug!("Result: {:?}", expensive_computation());
```

Here, `expensive_computation()` runs even if debug logging is off.

**Good (guarded):**

```rust
if log::log_enabled!(log::Level::Debug) {
    log::debug!("Result: {:?}", expensive_computation());
}
```

Now, `expensive_computation()` only runs if debug logging is actually enabled.

---
## **2. Assertions**

Rust has **two flavors of assertions**:

- **assert!** → always runs, in both debug and release builds.    
    - Use this for _safety-critical_ conditions that should _never_ be violated.
- **debug_assert!** → runs only in debug builds, skipped in release.
    - Use this for _development checks_ that help catch bugs but aren’t needed in production.

**Example:**

```rust
fn divide(a: i32, b: i32) -> i32 {
    debug_assert!(b != 0); // only checks in debug builds
    a / b
}
```

✅ In release builds, the check disappears → zero runtime overhead.

✅ In debug builds, you still get the safety net.

---
## **3. Structured Logging**

Instead of spamming `println!`, use crates:

- [log](https://docs.rs/log/) → common logging API    
- [env_logger](https://docs.rs/env_logger/) → simple logger backend
- [tracing](https://docs.rs/tracing/) → structured, async-friendly logging

```rust
use log::{info, debug};

fn main() {
    env_logger::init();

    info!("Starting app...");
    debug!("Debug info: {:?}", (1, 2, 3));
}
```

Run with:

```bash
RUST_LOG=debug cargo run
```

---
## **4. Rule of Thumb**

- Use `assert!` only for invariants that _must_ always hold (safety).    
- Use `debug_assert!` for performance-sensitive assertions.
- Guard expensive logging calls with `log_enabled!`.
- Prefer structured logging (log / tracing) over manual `println!`.

---