
- **val (value)** → Immutable (read-only).
    - Assigned once, cannot be reassigned.        
    - Like `final` in Java.

```kotlin
val languageName: String = "Kotlin"
// languageName = "Java" ❌ (error)
```

- **var (variable)** → Mutable (read-write). 
    - Can be reassigned multiple times.

```kotlin
var count: Int = 10
count = 15 ✅
```

---
## **Key Points**

- **Mutability:** `val` = immutable, `var` = mutable
- **Reassignment:** `val` ❌, `var` ✅
- **Use cases:**
    - Prefer `val` → safer, predictable, better for concurrency
    - Use `var` → only when reassignment is required

---
