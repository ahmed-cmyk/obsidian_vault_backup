
## **General Facts**

- Heap allocations are **moderately expensive**:
    
    - Global lock        
    - Data structure manipulation
    - Possible system calls
    
- Small allocations are **not necessarily cheaper** than large ones.
- Performance can often be improved by avoiding allocations.

---

## **Profiling Allocations**

- If profiler shows malloc/free hot → reduce allocation rate or try alternative allocator.
- **DHAT** (Linux & some Unix) is excellent for analyzing allocations:
    - Identifies hot sites, size, lifetime, and access frequency.
    - Example: reducing allocations by **10 per million instructions** → ~1% perf improvement in rustc.

---
## **Key Types & Their Allocation Behaviors**

### **Box**

- Simplest heap-allocated type.
- Used when storing a T on the heap.
- Sometimes box struct fields to reduce overall type size.
- Otherwise, not much optimization scope.

### **Rc / Arc**

- Like Box, but with **two reference counts**.    
- Pros: share values, reduce memory usage.
- Cons: can increase allocations if rarely shared.
- **Cloning** → no new allocation, just increments ref count.

### **Vec**

- 3 fields: length, capacity, pointer.    
- Elements always heap-allocated if nonzero-sized.
- Growth:
    - Default strategy: 0 → 4 → 8 → 16 → 32 → 64 → …
    - Efficient general case, but can waste space.
    
- Optimizations:
    - Use Vec::with_capacity / reserve / reserve_exact.
    - Use shrink_to_fit to reduce wasted space.

#### **Short Vecs**

- **SmallVec** (from smallvec crate): stores N elems inline before allocating.    
    - Reduces allocations but may be slower for normal ops.
    - If N is too big → larger type size, slower copies.
    
- **ArrayVec** (from arrayvec crate): no fallback allocation.
    - Faster if max size known.

#### **Long Vecs**

- Use with_capacity if you know min/max size.    
- Avoids multiple reallocations.

### **String**

- Backed by heap-allocated bytes, like `Vec<u8>`.    
- Optimizations:
    - with_capacity to pre-allocate.
    - **SmallString** (smallstr crate) or **SmartString** (smartstring crate).
        - SmartString avoids heap alloc for < 24 bytes (on 64-bit).
            
- **format! macro** always allocates.
    - Use string literals or format_args! if possible.

### **Hash Tables**

- HashMap / HashSet behave like Vec re: allocation.    
- Single contiguous heap allocation, grows as needed.
- with_capacity helps avoid reallocations.

---
## **Copying & Ownership**

### **clone**

- Usually allocates (e.g. Vec).
- Exception: Rc/Arc → increments ref count only.
- Use clone_from to reuse allocations where possible.
- Watch for unnecessary clone calls (profiling helps).

### **to_owned**

- Often allocates (e.g. &str → String).    
- Can avoid by storing references (&str) instead of owned (String).
- Trade-off: requires lifetimes, more complex code.

### **Cow (Clone on Write)**

- Holds **borrowed OR owned** data.    
- Great when:
    - Mixing borrowed + owned (&str + String).
    - Mostly-read-only borrowed data that sometimes needs mutation.
- Avoids unnecessary allocations.

---
## **Reusing Collections**

- Reuse Vecs instead of creating new ones.
- Example:
    - Create Vec outside loop → call clear() each iteration.
    - Or keep reusable collections inside a struct.

---
## **File Reading**

- `BufRead::lines` → allocates a String per line.
- Alternative: use reusable buffer with read_line.
    - Only reallocates when buffer too small.
    - Works if code can operate on &str instead of String.    

---
## **Alternative Allocators**

- Swap allocator to improve heap performance **without code changes**.
- Example: jemalloc, mimalloc, etc.

---
## **Avoiding Regressions**

- Use **dhat-rs** tests to ensure allocation counts don’t regress.

---
