
## **What is the jobserver?**

- A mechanism used by **build systems** (e.g. GNU Make) to coordinate **parallel jobs** across child processes.
- Ensures that when make -jN (or similar) runs, child tools (like rustc) don’t exceed the available parallel slots.

---
## **How rustc interacts with the jobserver**

- rustc can **take advantage of parallelism** internally (e.g., parallel codegen, parsing).
- If MAKEFLAGS contains a **jobserver descriptor** (e.g., --jobserver-auth=3,4), rustc will attempt to use it.
- If no jobserver is passed, rustc chooses its own number of jobs (usually based on CPU cores).

---
## **Warnings and misconfigurations**

- Since **Rust 1.76.0**, rustc warns if:
    - A jobserver appears in MAKEFLAGS but can’t be accessed.
    - Example:

```
MAKEFLAGS=--jobserver-auth=3,4 rustc main.rs
```

- - → might fail with _Bad file descriptor (os error 9)_.
- Cause: build system incorrectly passes jobserver info to child processes.

---
## **Integration tips for build systems**
  
**GNU Make**

- Use **recursive make** (+ prefix) to properly forward the jobserver:

```
x:
	+@echo 'fn main() {}' | rustc -
```

- If not recursive, jobserver gets broken and warnings appear.
- For $(shell ...) calls, clear MAKEFLAGS if you don’t want jobserver inheritance:

```
S := $(shell MAKEFLAGS= rustc --print sysroot)
```


**CMake**

- From **CMake 3.28+**:
    
    Use JOB_SERVER_AWARE TRUE to integrate directly.
    

```rust
add_custom_target(x JOB_SERVER_AWARE TRUE
  COMMAND echo 'fn main() {}' | rustc -
)
```

- Older versions (3.22+):
    
    Workaround by forcing recursive Make:
    

```rust
add_custom_target(x
  COMMAND DUMMY_VARIABLE=$(MAKE) echo 'fn main() {}' | rustc -
)
```

---
## **Why it matters for you**

- Normally **Cargo handles this** → you rarely touch jobserver settings directly.    
- Important if:
    - You integrate rustc in a **custom build system** (Make, Ninja, CMake).
    - You debug **parallel build issues** (jobs not scaling, unexpected warnings).
- Helps understand why cargo build -jN behaves smoothly — Cargo manages the jobserver correctly.

---