
- **Goal**: Identify “hot” code (frequently executed, worth optimizing).  

---
## Profilers

- **General-purpose**  
  - `perf` (Linux) → view with Hotspot / Firefox Profiler  
  - `Instruments` (macOS, Xcode)  
  - Intel VTune (Win/Linux/macOS)  
  - AMD μProf (Win/Linux)  

- **Sampling / Flame graphs**  
  - `samply` (Mac/Linux/Win, outputs Firefox Profiler data)  
  - `flamegraph` (Linux + DTrace: macOS, BSDs, maybe Win)  

- **Instruction-level / cache analysis**  
  - `Cachegrind`, `Callgrind` (Linux/Unix)  

- **Heap / memory**  
  - `DHAT` (Linux/Unix, allocations, memcpy hot spots)  
  - `dhat-rs` (cross-platform, less powerful, needs code changes)  
  - `heaptrack`, `bytehound` (Linux)  

- **Other**  
  - `counts` – ad-hoc, `eprintln!` + frequency analysis (all platforms)  
  - `Coz` – causal profiling (Linux, Rust support via coz-rs)  

---
## Debug Info

Add source-line debug info in release builds:  

```toml
[profile.release]
debug = "line-tables-only"
```

- Std lib lacks debug info by default:
    - Build your own (debuginfo-level = 1 in bootstrap.toml)
    - Or use build-std (but filenames won’t resolve for source-based profilers)

---
## **Frame Pointers**

- Needed for accurate stack traces.
- Enable with:

```bash
RUSTFLAGS="-C force-frame-pointers=yes" cargo build --release
```

- Or in config.toml:

```toml
[build]
rustflags = ["-C", "force-frame-pointers=yes"]
```

---
## **Symbol Demangling**

- Rust uses mangled names (_ZN..., _R...).
- Tools: rustfilt to demangle.
- Use **v0 mangling format**:

```bash
RUSTFLAGS="-C symbol-mangling-version=v0" cargo build --release
```

```toml
[build]
rustflags = ["-C", "symbol-mangling-version=v0"]
```

---
