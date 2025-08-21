
- **Definition**: Compare performance of programs (same task, or different versions).
- **Steps**:
    1. Choose **workloads**
        - Real-world inputs (best)
        - Also: [microbenchmarks](https://stackoverflow.com/questions/2842695/what-is-microbenchmarking), [stress tests](https://en.wikipedia.org/wiki/Stress_testing_$begin:math:text$software$end:math:text$)
        
    2. Choose **runner/metrics**
        - Rust nightly [built-in benchmarks](https://doc.rust-lang.org/nightly/unstable-book/library-features/test.html)
        - [Criterion](https://github.com/bheisler/criterion.rs), [Divan](https://github.com/nvzqz/divan) – advanced
        - [Hyperfine](https://github.com/sharkdp/hyperfine) – general purpose
        - [Bencher](https://github.com/bencherdev/bencher) – continuous (CI)
        - Custom harnesses (e.g. [rustc-perf](https://github.com/rust-lang/rustc-perf/))

---
## Metrics & Measurement
  
- **Metric choice = program type**  
  - Batch vs. interactive → different metrics matter  
- **Wall-time**: intuitive (user-perceived) but high variance  
  - Affected by small memory layout changes  
- **Alternatives**: cycles, instruction counts → lower variance  
- **Summarizing multiple workloads**: no single best method  
- **Reality check**:  
  - Benchmarking is hard  
  - Imperfect > none  
  - Start simple, refine as you learn
 
---