
## **1. Baseline**

- **Rust is fast by default** → avoid obvious pitfalls (e.g., don’t benchmark in _debug_ builds).
- Compared to **Python/Ruby/Java/C#**, idiomatic Rust usually wins in **speed + memory efficiency**.

---
## **2. When to Optimize**

- Optimized code is **harder to write/maintain** → only optimize **hot code paths** (identified via profiling).
- Most **big gains** come from **better algorithms or data structures**, not micro-optimizations.

---
## **3. Hardware Awareness**

- Write code that plays well with **modern CPUs**:
    - Minimize **cache misses**.
    - Reduce **branch mispredictions**.

---
## **4. Small Wins Compound**

- Many small speedups accumulate.
- Often easier to **eliminate silly slowdowns** than to invent clever tricks.

---
## **5. Profiling**

- Use **multiple profilers** — each has different strengths.
- When a hot function is found, two strategies:
    1. Make it faster.
    2. Call it less.

---
## **6. Common Optimization Heuristics**

- **Lazy/on-demand computations** → avoid unnecessary work.
- **Special-case handling**:
    - Optimize for **0, 1, 2 elements** if small sizes dominate.
    - Handle **common cases first** (based on frequency data).
- **Compact representations**:
    - Use compression or fallback tables for rare/unusual cases.
- **Caches for locality**:
    - Tiny front-cache for hot lookups can yield big wins.

---
## **7. Maintainability**

- Optimized code can look “weird.”
- Leave **profiling-informed comments** (e.g., _“99% of the time this vector has ≤1 element”_).

---