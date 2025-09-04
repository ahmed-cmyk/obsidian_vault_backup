
## **Inspecting Machine Code**

- For **very hot code paths**, it may be worth looking at the **generated machine code** to check for inefficiencies.    
    - **Example:** unnecessary bounds checks or redundant instructions.

**Tools:**

- [Compiler Explorer](https://godbolt.org/) → great for **small snippets**.    
- [cargo-show-asm](https://github.com/pacak/cargo-show-asm) → lets you see the assembly for **full Rust projects**.

### **SIMD and Intrinsics**

- The `core::arch` module provides **architecture-specific intrinsics**, including SIMD instructions.
- SIMD can accelerate **tight loops, vector math, and data processing**.

### **Key Point:**

- These techniques are usually a **last-mile optimization**:
    - First make sure algorithmic and structural performance issues are fixed.
    - Then use machine-level inspection to **squeeze out extra speed**.

---