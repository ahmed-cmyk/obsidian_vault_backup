
It is worth reading through the documentation for common standard library types—such as `Vec`, `Option`, `Result`, and `Rc`/`Arc`—to find functions that can improve both performance and readability.

It’s also worth knowing about **high-performance alternatives** to synchronization primitives (Mutex, RwLock, Condvar, Once), such as those provided by the **parking_lot** crate.

---
## **`Vec`**

- **Creating zero-filled vectors:**

```rust
let v = vec![0; n];
```

- This is simple and usually faster than alternatives (resize, extend, or unsafe code) because the OS can assist with zero-initialization.
- **Element removal:**
    - Vec::remove → removes element at index i, shifts everything left → **O(n)**.
    - Vec::swap_remove → replaces element at index i with the last element → **O(1)** (does not preserve order).
- **Bulk removal:**
    - Vec::retain efficiently removes multiple items.
    - Other collections (String, HashSet, HashMap) have equivalent retain methods.

---
## **`Option` and `Result`**

- **Eager vs lazy error construction:**

```rust
let r = o.ok_or(expensive());      // always evaluates expensive()
let r = o.ok_or_else(|| expensive()); // evaluates expensive() only if needed
```

- **General rule:** use the _else variants when the fallback is **expensive**.
- **Other similar patterns:**
    - Option::map_or → map_or_else
    - Option::unwrap_or → unwrap_or_else
    - Result::or → or_else
    - Result::map_or → map_or_else
    - Result::unwrap_or → unwrap_or_else

---
## **`Rc `/ `Arc`**

- **Clone-on-write semantics:**
    - Rc::make_mut / Arc::make_mut → obtain a mutable reference.
    - If refcount > 1, it **clones** the inner value to guarantee unique ownership.
    - Otherwise, modifies the original value directly.

👉 Useful in rare cases but extremely powerful when needed.

---
## **Synchronization Primitives**

- **Standard types:** Mutex, RwLock, Condvar, Once.
- **High-performance alternatives:** The [parking_lot](https://docs.rs/parking_lot) crate.
    - APIs/semantics are similar but not identical.
    - Parking_lot used to be _always faster_, but stdlib has improved a lot.
    - Always **measure before switching**.
- **Pitfall:** If you decide to universally use parking_lot, you may accidentally mix in stdlib types.
    - **Solution:** Use **Clippy lints** to catch mistakes.

---