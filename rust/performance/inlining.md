
Entry to and exit from hot, uninlined functions often accounts for a non-trivial fraction of execution time. Inlining these functions removes these entries and exits and can enable additional low-level optimizations by the compiler. In the best case the overall effect is small but easy speed wins.

There are four inline attributes that can be used on Rust functions:

- **None** — The compiler decides if the function should be inlined. This depends on optimization level, function size, whether the function is generic, and if the inlining is across a crate boundary.    
- **#[inline]** — Suggests that the function should be inlined.
- **#[inline(always)]** — Strongly suggests that the function should be inlined.
- **#[inline(never)]** — Strongly suggests that the function should _not_ be inlined.

Inline attributes do not guarantee that a function is or isn’t inlined, but in practice #[inline(always)] almost always results in inlining.

Inlining is **non-transitive**. If function f calls function g and you want both to be inlined together at a call site of f, then both must be marked with inline attributes.

---
## **Simple Cases**

Best candidates for inlining:

1. Very small functions.    
2. Functions with a single call site.

The compiler often inlines these automatically, but sometimes attributes are needed.

**Profiler tip:** Use **Cachegrind**.

- A function has been inlined if its first and last lines have no event counts.    
- Example:

```rust
#[inline(always)]
fn inlined(x: u32, y: u32) -> u32 {
    eprintln!("inlined: {} + {}", x, y);
    x + y
}

#[inline(never)]
fn not_inlined(x: u32, y: u32) -> u32 {
    eprintln!("not_inlined: {} + {}", x, y);
    x + y
}
```

> **Note:** Always measure after adding inline attributes. Effects can be unpredictable.

- Sometimes no effect.
- Sometimes performance worsens.
- Cross-crate inlining may increase compile times due to duplicated internal representations.

---
## **Harder Cases**

Problem: A large function with multiple call sites, but only one hot site.

- Goal: Inline at hot site, but avoid bloat at cold sites.    

**Solution:** Split into always-inlined and never-inlined variants.

```rust
// Use this at the hot call site
#[inline(always)]
fn inlined_my_function() {
    one();
    two();
    three();
}

// Use this at the cold call sites
#[inline(never)]
fn uninlined_my_function() {
    inlined_my_function();
}
```

---
## **Inlining**

**Definition:**

Inlining is when the compiler replaces a function call with the body of the function itself. Instead of generating a call/return sequence, the function’s code is directly “pasted” at the call site.

**Benefits:**

- Eliminates function call overhead (no stack push/pop, no jump/return).    
- Enables further optimizations like constant folding, loop unrolling, and dead-code elimination.
- Speeds up execution at _hot call sites_ (functions used very frequently).

**Disadvantages:**

- Increases binary size due to code duplication (code bloat).
- Can hurt performance because of instruction cache misses.
- Slows compile times (especially across crates).
- May sometimes reduce performance if it prevents better optimizations.

---
## **Hot vs Cold Call Sites**

- **Hot call site:** A location in code where a function is called extremely often (e.g., inside an inner loop). Inlining here usually improves performance.
- **Cold call site:** A location where the function is rarely executed (e.g., initialization, error handling). Inlining here usually wastes space without benefit.

**Practical strategy:**

- Inline functions at hot call sites.
- Avoid inlining at cold call sites to prevent code bloat.
- Rust gives you control with attributes like #[inline], #[inline(always)], and #[inline(never)].

---
