# System Design: Availability vs Consistency

In a distributed computer system, you can only support two of the following guarantees:

1. **Consistency:** Every read receives the most recent write or an error.
2. **Availabilty:** Every request receives a response, without guarantee that it contains the most recent version of the information.
3. **Partition Tolerance**: The system continues to operate despite arbitrary partitioning due to network failures.

*Networks aren't reliable, so you'll need to support partition tolerance. You'll need to make a software tradeoff between consistency and availability.*

---
## Patterns

### CP - consistency and partition tolerance

Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes.

### AP - availability and partition tolerance

Response return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.

AP is a good choice when the business needs to allow for eventual consistency or when the system needs to continue working despite external errors.

---
## Example: Atomic reads and writes

### Bank Account Transfers

Imagine a simple banking system where users can transfer money between accounts.

### Scenario: Alice transfers $100 to Bob

This involves two steps:
1. **Withdraw $100 from Alice’s account**
2. **Deposit $100 into Bob’s account**
⠀
If these steps aren’t **atomic** (i.e., they don’t happen as one indivisible operation), you can run into serious problems:

### Without Atomicity:
* System crashes after step 1 (Alice’s account debited)
* Step 2 never happens (Bob doesn’t get the money)
* **Result: $100 disappears from the system**

### With Atomicity:
* Both steps are treated as one **atomic transaction**
* Either **both succeed** or **both fail**
* System ensures **data consistency** even during crashes or concurrent access

### Why Atomic Reads Matter Too

Imagine Bob checks his balance **while** the transfer is in progress.
* If he reads it **between** step 1 and 2:
  * He might see an inconsistent state (e.g., money deducted from Alice, but not yet added to him)
* With **atomic reads**, the system guarantees Bob either sees the **before-transfer** state or the **after-transfer** state — never the **in-between**

### Summary

Banking systems are a classic case where **atomic reads and writes** are critical to ensure:
* **Correctness**
* **Consistency**
* **No lost or duplicated money**

⠀
This same principle applies to inventory systems, booking platforms, payment gateways, etc.

---

#backend