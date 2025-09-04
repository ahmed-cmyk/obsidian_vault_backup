
Shrinking types that are frequently instantiated can improve **performance** and **memory usage**.

---
## **Why it matters**

- Reduces **peak memory usage**
- Reduces **memory traffic** and **cache pressure**
- Avoids unnecessary memcpy (Rust copies values > 128 bytes with memcpy)

👉 Use **DHAT** (heap profiler) to:

- Identify hot allocation points & types.
- Enable **copy profiling** to locate expensive memcpy calls.

---
## **Measuring Type Sizes**

- `std::mem::size_of` → returns size in bytes.
- For full **layout info**:

```bash
# Cargo + nightly
RUSTFLAGS=-Zprint-type-sizes cargo +nightly build --release

# rustc + nightly
rustc +nightly -Zprint-type-sizes input.rs
```

### **Example**

```rust
enum E {
    A,
    B(i32),
    C(u64, u8, u64, u8),
    D(Vec<u32>),
}
```

Output (excerpt):

```
print-type-size type: `E`: 32 bytes, alignment: 8 bytes
print-type-size     discriminant: 1 bytes
print-type-size     variant `D`: 31 bytes
print-type-size     variant `C`: 23 bytes
print-type-size     variant `B`: 7 bytes
print-type-size     variant `A`: 0 bytes
```

Shows:

- Total size & alignment
- Discriminant size
- Variant sizes (sorted by largest)
- Field ordering, padding, alignment

👉 For a compact view: use [**top-type-sizes**](https://crates.io/crates/top-type-sizes).

---
## **Ways to Shrink Hot Types**

### **1. Smaller Enums**

- Box large variants to reduce enum size.

```rust
// Before
type LargeType = [u8; 100];
enum A {
    X,
    Y(i32),
    Z(i32, LargeType),
}

// After
enum A {
    X,
    Y(i32),
    Z(Box<(i32, LargeType)>),
}
```

✅ Trade-off: extra heap allocation for rare variants.

⚠️ Slightly less ergonomic in match.

### **2. Smaller Integers**

- Replace usize with u32, u16, or u8 if safe.
- Cast back to usize at usage points.

### **3. Boxed Slices**

- `Vec<T>` stores **3 words**: len, capacity, ptr.
- `Box<[T]>` stores **2 words**: len, ptr.
- Drops excess capacity (may reallocate).

```rust
let v: Vec<u32> = vec![1, 2, 3];
let bs: Box<[u32]> = v.into_boxed_slice();
```

Convert back:

```rust
let v2: Vec<u32> = bs.into_vec();
```

### **4. ThinVec**

- From [thin_vec](https://crates.io/crates/thin-vec).
- Functionally like `Vec<T>`.
- `size_of::<ThinVec<T>>() == 1` word.
- Good for:
    - Vectors that are often empty.
    - Shrinking the largest variant of an enum.

---
## **Preventing Regressions**

Use **static assertions** to enforce type size.

```rust
#[cfg(target_arch = "x86_64")]
static_assertions::assert_eq_size!(HotType, [u8; 64]);
```

⚠️ Type sizes differ by platform → use #[cfg(...)] guards.

---

✅ **Summary**

- Profile with **DHAT** before optimizing.
- Shrink types via **boxing**, **smaller ints**, **boxed slices**, or **ThinVec**.
- Guard against regressions with **static assertions**.

---
