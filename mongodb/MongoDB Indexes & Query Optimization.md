# MongoDB: Indexes & Query Optimization

Indexes in MongoDB allow you to efficiently query and sort data by creating a data structure that maps field values to document locations. Without indexes, MongoDB must perform a **collection scan**, which is slow for large datasets.

---
## Why Use Indexes?

✅ Improve query performance by orders of magnitude.
✅ Enable efficient sorting.
✅ Support unique constraints.
✅ Power advanced features like text search and geospatial queries.

---
## Creating Indexes

Create an index on a single field:

```js
db.users.createIndex({ name: 1 });
```

> `1` for ascending, `-1` for descending.

Unique index:

```js
db.users.createIndex({ email: 1 }, { unique: true });
```

Compound index (multiple fields):

```js
db.users.createIndex({ age: 1, name: -1 });
```

---
## Index Types

### Single Field Index

Basic index on a single field.

### Compound Index

Indexes multiple fields in a specific order, useful for queries that filter or sort on more than one field.

### Multikey Index

Created automatically when indexing an array field. Each element of the array is indexed.

### Text Index

Supports text search within string fields:

```js
db.articles.createIndex({ content: "text" });
```

Search with:

```js
db.articles.find({ $text: { $search: "mongodb indexing" } });
```

### Geospatial Index

For queries on location-based data:

* `2d`: for flat geometry.
* `2dsphere`: for spherical geometry.

```js
db.places.createIndex({ location: "2dsphere" });
```

### Hashed Index

For sharding, indexes a field’s hash value:

```js
db.users.createIndex({ userId: "hashed" });
```

---
## Viewing & Managing Indexes

List indexes:

```js
db.users.getIndexes();
```

Drop an index:

```js
db.users.dropIndex("name_1");
```

Drop all indexes:

```js
db.users.dropIndexes();
```

---

## Query Optimization Tips

* Always use indexes for fields frequently queried or sorted.
* Avoid scanning the entire collection whenever possible.
* Use `explain()` to analyze query performance:

```js
db.users.find({ name: "Alice" }).explain("executionStats");
```

* Be cautious with large `$in` or regex queries — these can still be expensive even with indexes.
* Use covered queries where possible — queries that can be answered using only the index, avoiding the need to fetch the document.

---
## Notes

* Too many indexes can slow down writes, as each index must be updated.
* Pick indexes based on your most common query patterns.
* Monitor slow queries and adjust indexes accordingly.

---

#mongodb