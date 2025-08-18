# MongoDB: Sharding & Scalability

**Sharding** is the process of distributing data across multiple servers (or clusters) to handle very large datasets and high throughput workloads.

It allows MongoDB to scale **horizontally** — adding more machines instead of making a single machine more powerful.

---

## Why Sharding?

As your data grows beyond the capacity of a single server:

* Storage may no longer fit on one machine.
* Read and write operations become a bottleneck.
* Memory and CPU limitations are reached.

Sharding solves these by **splitting the data across shards** and distributing the load.

---
## Sharding Components

| Component | Description |
|---|---|
| **Shard** | A MongoDB server or replica set that holds a portion of the data. | 
| **Config Servers** | Store metadata and configuration about the cluster (at least 3 for reliability). | 
| **mongos** | A query router that directs requests to the appropriate shard(s). | 

---
## How it Works

1️⃣ Data is partitioned into **chunks**, based on a **shard key**.
2️⃣ Each chunk is assigned to a shard.
3️⃣ When a query comes in, `mongos` uses the metadata to route it to the correct shard(s).
4️⃣ If the shard key is well-chosen, the workload is balanced evenly.

---
## Shard Key

> **The shard key is critical!**

* The field(s) used to split data into chunks.
* Should be chosen to distribute data and load evenly.
* Bad shard keys can lead to unbalanced shards (a *hot shard*).

For example:

```js
sh.shardCollection("mydb.mycollection", { userId: 1 })
```

Here, `userId` is used as the shard key.

---
## Benefits of Sharding

✅ Horizontal scaling — add more machines as needed.
✅ High write throughput — writes can happen on multiple shards in parallel.
✅ Large dataset support — no size limits of a single machine.
✅ Read scaling — queries can be served from all shards if appropriate.

---
## Best Practices

* Choose a **high-cardinality, evenly distributed shard key**.
* Monitor the cluster for hot shards and re-balance if needed.
* Keep config servers healthy and backed up — they are critical.
* Ensure your application is designed to tolerate distributed queries (which may be slower).

---
## Common Commands

### Enable Sharding on a Database

```js
sh.enableSharding("mydb")
```

### Shard a Collection

```js
sh.shardCollection("mydb.mycollection", { userId: 1 })
```

### View Cluster Status

```js
sh.status()
```

---

Sharding allows MongoDB to power huge applications like social networks, analytics platforms, and IoT systems, where massive scale and distributed architecture are required.

---

#mongodb