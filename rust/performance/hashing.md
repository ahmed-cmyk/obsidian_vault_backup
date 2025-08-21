
## **Hashing in Rust (HashSet & HashMap)**

- **Default hasher:**
    
    - Rust does not specify the default hashing algorithm.        
    - Currently, the default is **SipHash 1-3**.    
    - Characteristics:
        - High-quality → strong collision resistance.
        - Relatively slow, especially for short keys like integers.
            
- **Performance considerations:**
    
    - If profiling shows hashing is a bottleneck **and** HashDoS is not a concern, switching to a faster hasher can give significant speed improvements.

---
## **Alternative Hash Implementations**

- **rustc-hash**
    
    - Provides FxHashSet and FxHashMap.        
    - Very fast, especially for integer keys.
    - Low-quality hash, but outperforms others in rustc.
    - ⚠️ Older fxhash crate exists but is less maintained.
    
- **fnv**
    
    - Provides FnvHashSet and FnvHashMap.
    - Higher-quality hashing than rustc-hash.
    - Slower than rustc-hash.
    
- **ahash**
    
    - Provides AHashSet and AHashMap.
    - Can use **AES instructions** on supported processors for extra speed.

---
## **Observed Results in** 

### **rustc**

- Switching from **fnv → fxhash**: **up to 6% speedup**.
- Switching from **fxhash → ahash**: **1–4% slowdown**.
- Switching from **fxhash → default (SipHash)**: **4–84% slowdown**.

---
## **Practical Notes**

- If you decide to universally adopt an alternative hasher (like FxHashMap):
    
    - Easy to accidentally still use HashMap.        
    - ✅ Use **Clippy lints** to catch unintended uses.
    
- **nohash_hasher**
    
    - Useful if your type doesn’t need hashing (e.g., a newtype wrapping random-ish integers).
    - In such cases, value distribution ≈ hash distribution, so hashing adds no benefit.
    
- **Hash function design** is complex → see ahash docs for deeper discussion.

---
## **When to Use Which Hasher**

- **Default (HashMap, HashSet)**
    
    - ✅ Best when **security (HashDoS resistance)** is a concern.        
    - ✅ Good default if you don’t know what distribution your keys will have.
    - ❌ Slower for short keys (like integers).
    
- **rustc-hash (FxHashMap, FxHashSet)**
    
    - ✅ Fastest option for integer keys, symbols, or compiler-like workloads.
    - ❌ Poor quality hash → avoid for untrusted input.
    - ⚠️ Requires discipline (or Clippy) to avoid accidentally mixing with HashMap.
    
- **fnv (FnvHashMap, FnvHashSet)**
    
    - ✅ Higher-quality hash than rustc-hash.
    - ❌ Slower, so usually not chosen unless you care about fewer collisions.
    
- **ahash (AHashMap, AHashSet)**
    
    - ✅ Balanced: faster than default, higher quality than rustc-hash.
    - ✅ Uses **hardware acceleration** (AES) when available.
    - ❌ Slightly slower than rustc-hash in compiler-like workloads.
    
- **nohash_hasher**
    
    - ✅ Best if your keys are already uniformly distributed (e.g., random IDs).
    - ✅ Eliminates redundant hashing.
    - ❌ Only works with integer-like key types.

---
