
While runtime speed matters, **developer productivity** is also affected by how long your project takes to compile. Rust’s compiler is known to be strict and heavy, but there are ways to minimize wait time.

---
## **1. Build Configuration Tricks**

(Already covered in _Minimizing Compile Times_ section — things like incremental compilation, fewer dependencies, etc.)

---
## **2. Visualizing Compilation**

- Run:

```bash
cargo build --timings
```

- Produces an **HTML report** → shows a **Gantt chart** of crate compilation.
- Helps you see:
    - Where parallelism is being used.
    - Which crates are bottlenecks.
    - If any large crates serialize compilation and should be **split**.

---
## **3. LLVM IR (Intermediate Representation)**

Rust uses **LLVM** as the backend.

LLVM optimization time can balloon if your code generates **too much IR**.

### **Diagnosing IR bloat**

- Use:

```bash
cargo llvm-lines
```

- Tells you which Rust functions produce the most IR.
- **Generics** are the usual culprit, since each instantiation creates new IR.

---
## **4. Strategies to Reduce IR Bloat**

### **a) Make functions smaller**

- **Example:** trim logic inside generics.

### **b) Split generic and non-generic parts**

- Move the **non-generic part** into a helper function (non-generic = only compiled once).
- **Example:** `std::fs::read`:

```rust
pub fn read<P: AsRef<Path>>(path: P) -> io::Result<Vec<u8>> {
    fn inner(path: &Path) -> io::Result<Vec<u8>> {
        let mut file = File::open(path)?;
        let size = file.metadata().map(|m| m.len()).unwrap_or(0);
        let mut bytes = Vec::with_capacity(size as usize);
        io::default_read_to_end(&mut file, &mut bytes)?;
        Ok(bytes)
    }
    inner(path.as_ref())
}
```

### **c) Avoid excessive generic combinators**

- Utility methods like `Option::map` or `Result::map_err` → get instantiated many times.
- Replacing them with match expressions can sometimes reduce compile time.

---
## **5. Side Effects**

- These changes usually give **small improvements**, but occasionally **big wins**.
- They can also **reduce binary size**, not just compile time.

---

👉 In short:

- **Measure first** (`cargo build --timings`, `cargo llvm-lines`).
- **Tweak generics & utility calls** where IR bloat is excessive.
- Expect small but meaningful compile-time gains.

---
