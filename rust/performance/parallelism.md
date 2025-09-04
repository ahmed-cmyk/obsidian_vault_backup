
- Rust provides **safe parallel programming** support, which can lead to **major performance gains**.
- The best parallelism strategy depends heavily on your program’s **design** and **workload characteristics**.

---
## **Crates to Explore**

- [rayon](https://docs.rs/rayon/latest/rayon/) → ergonomic **data parallelism** (parallel iterators, maps, filters, reductions).    
- [crossbeam](https://docs.rs/crossbeam/latest/crossbeam/) → **low-level concurrency tools** (channels, scoped threads, memory management).

---
## **Key Point**

- Parallelism can give large speedups, but also adds **complexity**.
- Always profile before and after — sometimes parallelism adds overhead that outweighs the benefit.

---