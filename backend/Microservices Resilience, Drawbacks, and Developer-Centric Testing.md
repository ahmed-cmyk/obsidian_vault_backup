# Microservices: Resilience, Drawbacks, and Developer-Centric Testing

*Based on a dissertation on resilient microservice applications* 

---
## **Benefits of Microservices**

* **Organizational and Development Velocity Scaling**:Microservices accelerate development by enabling independent deployment and ownership. Small teams can iterate faster without waiting on global coordination, making the architecture highly suitable for large organizations.
* **Modularization**: Clear service boundaries encourage modular thinking and better software engineering practices. Modularization directly boosts developer productivity and reduces cognitive load.

---
## **Challenges and Drawbacks**

### **Partial Failure**

* Microservices introduce **partial failure** risks—where some services may be unavailable even if the rest of the application is functioning.
* Developers must **explicitly account for these failures**, as traditional testing doesn’t usually catch them.

### **Complexity in Large-Scale Systems**

* **Difficult to Debug and Profile**: Fragmentation across services makes root cause analysis harder.
* **Tight Coupling via RPCs**: Services often develop interdependencies that violate the ideal of loose coupling.
* **Cross-Cutting Changes Are Expensive**: Implementing features that span many services requires coordination, re-releases, and increases failure risk.
* **Transactional Complexity**: Performing atomic transactions across multiple services is unreliable and complex.

### **Fault Injection and Chaos Testing Limitations**

* **Latent Defects Slip Through**: Traditional chaos testing often fails to reveal subtle bugs that only manifest in rare conditions.
* **High Cost of Testing**: With hundreds or thousands of services, it’s impractical to test every failure combination manually.
* **Ambiguity in Expected Behavior**: Metrics like “stream-starts-per-second” don’t catch logic bugs or silent failures.
* **Risk in Production Chaos Experiments**: Experiments may unintentionally degrade user experience.
* **Limited Frequency**: Due to cost and overhead, chaos experiments can’t be run per commit or daily.

### **Developer Experience Issues**

* **Unclear Change Points**: Developers often don’t know where to make a change due to poor observability and unclear service ownership.
* **Unrunnable Local Stacks**: Running the full system locally is difficult or impossible, hampering testing and iteration.
* **Inaccessible Realistic Data**: It’s hard to obtain or simulate realistic subsets of production data for local or integration testing.
* **Schema Evolution is Complex**: Shared or duplicated schemas make it hard to refactor safely across services.
* **Misconfigured Timeouts**: Unaligned timeouts across services lead to unexpected service failures and retries.
* **Latent Bugs & Silent Failures**: Bugs that only appear under specific failure conditions are hard to detect. Soft dependencies that degrade gracefully can obscure real problems.
* **Burden of Test Adaptation**: Developers must manually adapt “happy path” tests to account for faults, which is tedious and error-prone.

---
## **Author’s Contributions and Solutions**

### **Service-level Fault Injection Testing (SFIT)**

* Exhaustive fault injection technique that integrates directly with existing functional tests.
* Forces developers to **define expected behavior under failure** using *fault injection predicates*.
* Requires no separate specifications and works at the service level.

### **Distributed Execution Indexing (DEI)**

* **Unique, deterministic tracing** of every RPC, even under concurrency or non-determinism.
* Enables:
  * Fault injection
  * Deterministic replay
  * Advanced dynamic analysis (e.g., detecting service anti-patterns or “smells”)

### **Principled SFIT (p-SFIT)**

* A structured extension of SFIT that:
  * Helps developers adapt functional tests to include both hard and soft dependency failure handling.
  * Uses IDE plugins, visualizations, and structured recommendations to guide developers in diagnosing and encoding complex failure scenarios.
  * Supports graceful degradation testing with clarity.

### **Encapsulated Service Reduction (ESR)**

* Optimization that **reduces redundant test cases** by taking advantage of service graph structure (deep vs. wide).
* Focuses test coverage on critical interaction paths rather than brute force.

### **Industrial Evaluation**

* Research done in collaboration with **Foodly**, a major U.S. food delivery service.
* Resulted in **discovery of critical latent bugs** that had previously gone unnoticed in production.
* Demonstrates practical impact and scalability of the techniques.

---
## **Core Philosophy**

- **Developer-Centric Resilience**: Instead of relying on coarse-grained chaos engineering post-deployment, bring resilience testing into development pipelines.
- **Make Large Systems Easier to Build Than Small Ones**: Tools and processes should enable cross-cutting changes without re-releasing the entire stack.
- **Developer Convenience is Paramount**: Productivity, safety, and testability should be built into the architecture from day one.

---

#backend