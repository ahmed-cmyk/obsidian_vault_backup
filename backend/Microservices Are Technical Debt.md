# Microservices Are Technical Debt

> “You borrow against someone else’s productivity in the future.”
> 
> — Matt Ranney (Uber)

---
## **Benefits**

### **Uber’s Context**

* **Organizational Scalability**: Enabled Uber to grow from 200 to 2,500+ engineers by letting teams work independently, reducing bottlenecks from centralized decision-making.
* **Selective Deployments**: Easier to release or update specific parts of the system rather than the entire monolith.
* **Resource Optimization**: Ability to scale compute-heavy services (e.g., geospatial matching) independently.
* **Team Autonomy**: Developers could focus on their service boundaries without worrying about the entire application stack.

---

## **Challenges**

### **System Complexity**

* **Feature Development Slowed**: Making product changes required involvement from multiple tightly-coupled services.
* **Debugging and Profiling**: Harder due to fragmentation and unclear service ownership.
* **Tight Coupling via RPC**: Services intended to be loosely coupled ended up depending on each other’s API contracts and behaviors, causing cascading changes.

### **Cross-Cutting Concerns**

* **Expensive Cross-Service Features**: Implementing features like new product types (“Uber Puppies”) required coordination across routing, demand, user services, etc.
* **Atomicity Across Services**: Performing reliable, transactional operations over multiple RPCs proved extremely difficult.

### **Developer Experience**

* **Poor Local Dev Experience**:
  * Could not run the full microservices stack locally.
  * Testing required dealing with partial, stale, or anonymized datasets.

* **Onboarding Challenges**: Developers struggled to find where to make changes due to poor observability and documentation.
* **Schema Evolution Pain**: Data schemas were duplicated or shared across services, making versioning and refactoring hard and risky.

### **Classic Microservices Pitfalls Uber Faced**

* Database scaling
* Cross-language RPC frameworks
* Service discovery
* Internal DoS (self-inflicted through poor coordination)
* Circuit breakers
* Graceful degradation
* Distributed tracing

---
## **Solutions**

### **Dosa (Data-Oriented Service Architecture)**

* **Goal**: Make schema evolution safer and allow interaction with production data without risking it.

* **Approach**:
  * Developers use a library that communicates with a Dosa service, which handles schema negotiation at service startup.
  * Supports backward-compatible changes across services.
  * Introduced *Storage Scopes* (copy-on-write data layers) for safe testing and experimentation.
  * Helps decentralize access to user data (e.g., Populace service) while retaining control and composability.

### **Sagas (for Long-Lived Transactions)**

* **Problem**: Distributed transactions are hard to guarantee with RPC alone.
* **Solution**: Use *Sagas* — a coordinator issues actions to multiple services, each with a compensating rollback function.
* **Result**: Achieves *eventual consistency* across services rather than strict ACID properties.

### **Composable Event Processors (Future Direction)**

* **Vision**: Replace tight RPC coupling with asynchronous event-driven communication.

* **Mechanism**:
  * Services emit events (e.g., “Find nearby drivers”) instead of directly invoking other services.
  * An orchestration layer routes these to appropriate handlers.

* **Benefits**:
  * New logic (e.g., Uber Puppies) can be added as a plug-in without rewriting or redeploying existing services.
  * Reduces coordination overhead between teams.
  * Promotes scalability and flexibility at runtime.

* **Philosophy**: Events describe *intent*, not execution — services don’t assume which component handles them.

---
## **Philosophy and Principles**

* **Scale Should Improve Developer Experience**: Tools should make large systems easier to build and maintain than small ones.
* **Developer Productivity at the Core**: Dev-friendly tools and workflows are essential at any scale — better tooling can prevent premature re-architecture.
* **Cross-Cutting Change as Benchmark**: The ease of implementing features that touch many services (like “Uber Puppies”) is the true test of a good architecture.

---

#backend