
## **`collect` and `extend`**

- `Iterator::collect` converts an iterator into a collection like `Vec`, which requires allocation.
    - Avoid collecting if you’ll just iterate again.
    - Prefer returning `impl Iterator<Item = T>` from functions instead of `Vec<T>`.
    - Returning iterators may require lifetimes on return types.
    
- `extend` can extend an existing collection with items from an iterator.
    - More efficient than `collect` into a `Vec` and then appending.
    
- If writing a custom iterator, implement:
    - `Iterator::size_hint` (approximate bound)
    - `ExactSizeIterator::len` (exact count)
    - → helps `collect` and `extend` avoid redundant allocations.

---
## **Chaining**

- `chain` is convenient but slower for hot iterators.
    - Prefer a single iterator where possible.
- `filter_map` can be faster than `filter` → `map`.

---
## **Chunks**

- If chunk size **exactly divides** slice length:
    - Use `slice::chunks_exact` → faster than `slice::chunks`.
- If not exact:
    - Still use `chunks_exact` with `ChunksExact::remainder` (or handle leftovers manually).
- Related optimized iterators:
    - `slice::rchunks`, `slice::rchunks_exact`, `RChunksExact::remainder`
    - `slice::chunks_mut`, `slice::chunks_exact_mut`, `ChunksExactMut::into_remainder`  
    - `slice::rchunks_mut`, `slice::rchunks_exact_mut`, `RChunksExactMut::into_remainder`

---