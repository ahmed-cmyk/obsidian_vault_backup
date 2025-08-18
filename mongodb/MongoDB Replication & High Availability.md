# MongoDB: Replication & High Availability

**Replication** is the process of keeping identical copies of your data on multiple servers, ensuring your database remains highly available and fault-tolerant.

In MongoDB, replication is implemented through **Replica Sets**.

---
## What is a Replica Set?

A **Replica Set** is a group of MongoDB servers (nodes) that maintain the same dataset.

It consists of:

* **Primary Node**:
* Accepts all write operations. There is only one primary at a time.

* **Secondary Nodes**:
* Maintain copies of the primary’s data. Can serve read operations (if allowed).

* **Arbiter (optional)**:
* Participates in elections but does not store data. Useful when you need an odd number of voting members.

---
## How it Works

1️⃣ The primary logs all writes in its **Oplog** (operations log).
2️⃣ Secondaries continuously replicate the Oplog and apply changes to stay in sync.
3️⃣ If the primary goes down, an election is held and one of the secondaries becomes the new primary automatically.

---
## Benefits of Replication

✅ High Availability: If the primary fails, the replica set promotes a secondary.
✅ Read Scalability: You can configure reads from secondaries (eventually consistent).
✅ Data Redundancy: Multiple copies of your data increase durability.
✅ Disaster Recovery: If one data center is lost, replicas in another location can take over.

---
## Common Commands

### Initialize a Replica Set

```js
rs.initiate()
```

### Add a Secondary

```js
rs.add("hostname:port")
```

### Check Replica Set Status

```js
rs.status()
```

---
## Best Practices

* Always have an odd number of voting members to prevent election ties.
* Spread nodes across multiple availability zones or data centers.
* Use an arbiter only if you must (it doesn’t store data).

---
## Terminology Recap

| Term | Meaning |
|---|---|
| **Primary** | Node that accepts writes | 
| **Secondary** | Follows primary and replicates data | 
| **Oplog** | Log of changes applied to replicate | 
| **Election** | Process to choose a new primary if the current fails | 
| **Arbiter** | Votes in elections but stores no data | 

---

Replica sets are essential for production-grade MongoDB deployments. They ensure your application remains operational even when failures occur.

---

#mongodb