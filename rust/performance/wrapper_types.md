
## **Wrapper Types**

- Rust provides several **wrapper types** (e.g., `RefCell`, `Mutex`, `Arc<Mutex<T>>`) that enable interior mutability, thread-safety, and reference counting.
- However, accessing values inside wrappers incurs non-trivial overhead.

### **Guideline:**

- If **multiple wrapped values are usually accessed together**, it may be better to **group them into a single wrapper**.

**Example (less efficient):**

```rust
struct S {
    x: Arc<Mutex<u32>>,
    y: Arc<Mutex<u32>>,
}
```

**Example (better if accessed together):**

```rust
struct S {
    xy: Arc<Mutex<(u32, u32)>>,
}
```

#### **Key Point:**

- This reduces the **number of locks** or wrapper overhead.
- Benefit depends on **access patterns**:
    - If x and y are usually modified together → grouping helps.
    - If they’re independent most of the time → separate wrappers may be fine.

---
