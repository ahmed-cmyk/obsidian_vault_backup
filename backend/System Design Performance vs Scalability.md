# System Design: Performance vs Scalability

A service is scalable if it results in increased performance in a manner proportional to the resources added. 

> Generally increasing performance means serving more units of work, but it can also be to handle large units of work, such as when datasets grow.

---
## Scalability in distributed systems

In distributed systems there are other reasons for addings resources to a system; for example to *improve the reliability of the offered service*. Introducing redundancy is an important first line of defense against failures.

>An always-on service is said to be scalable if adding resources to facilitate redundancy does not result in a loss of performance.

---
## Why Is Scalability Hard?

Scalability must be part of a system’s **initial design**—it can’t be added later. Key challenges include:

* **Scaling isn’t just about adding hardware** — the system must be built so that added resources **actually improve performance**.
* **Redundancy must not hurt performance** — systems should handle duplicate components without slowing down.
* **Algorithms can fail at scale** — what works fine with small data or low traffic may become expensive when:
  * Request volume increases
  * Dataset size grows
  * More nodes are added

⠀
---
## The Challenge of Heterogeneity

When systems scale, they often end up with **non-uniform (heterogeneous) resources**, due to:

* Mixing old and new hardware
* Varying performance/storage capabilities
* Physical distance between resources
⠀
This causes problems because:

* Some nodes are **faster or larger** than others
* Algorithms expecting uniform performance may:
  * **Break** under real-world conditions
  * **Underuse** the best hardware

---

## Another way to look at performance vs scalability

- If you have a performance problem, your system is slow for a single user.
- If you have a scalability problem, your system is fast for a single user but slow under heavy load.

---

#backend