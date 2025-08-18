# MongoDB: Indexes & Performance Optimization

Indexes in MongoDB improve **query performance** by allowing the database to quickly locate the requested data without scanning every document in a collection.
Without an index, MongoDB must perform a **collection scan**, which is much slower.

---
## Why Use Indexes?

✅ Faster read queries.
✅ Support for sorting efficiently.
✅ Enable unique constraints.
✅ Make range queries and text searches possible.

---
## Types of Indexes

### 📄 Single Field Index

* Created on a single field.
* Improves queries that filter or sort by that field.

```
db.users.createIndex({ username: 1 })
```

`1` → ascending, `-1` → descending.

---
### 📄 Compound Index

* Covers multiple fields in a single index.
* Order of fields in the index matters!

```js
db.orders.createIndex({ userId: 1, createdAt: -1 })
```

---
### 📄 Multikey Index

* Created automatically when indexing an array field.

* Allows queries to match individual elements in the array.

```js
db.posts.createIndex({ tags: 1 })
```

---
### 📄 Text Index

* Supports text search within string fields.
* You can create a text index on one or more string fields.

```js
db.articles.createIndex({ content: "text", title: "text" })
```

Search:

```js
db.articles.find({ $text: { $search: "mongodb" } })
```

---
### 📄 Geospatial Index

* Supports queries for spatial data (e.g., maps, locations).
* `2dsphere` is the most common type.

```js
db.places.createIndex({ location: "2dsphere" })
```

---
### 📄 Hashed Index

* Useful for sharding with a hashed shard key.
* Ensures even distribution of data.

```js
db.users.createIndex({ userId: "hashed" })
```

---
## Performance Tips

> **Understand your workload and query patterns before creating indexes.**

✔ Only index the fields that are frequently used in queries, sorts, or for uniqueness.
✔ Keep indexes as small as possible to reduce memory usage.
✔ Use `explain()` to analyze query performance.

---
## Analyzing Queries

You can inspect how MongoDB executes a query using `explain()`:

```js
db.users.find({ username: "ahmed" }).explain("executionStats")
```

Look for:

* `COLLSCAN` → collection scan (bad for large collections).
* `IXSCAN` → index scan (good).
* `FETCH` → fetching the documents after locating them via index.

---
## Removing Indexes

```js
db.users.dropIndex("username_1")
```

Or drop all:

```js
db.users.dropIndexes()
```

---

Indexes are a key part of building **fast, scalable applications** in MongoDB. Over-indexing can hurt performance, so balance is important.

---

#mongodb