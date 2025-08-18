# MongoDB: Aggregation Framework & Pipelines

The **Aggregation Framework** allows you to perform powerful data transformations and analytics directly in MongoDB, processing data in a series of **stages**, called a **pipeline**.

Think of it as SQL’s `GROUP BY`, `ORDER BY`, and calculated columns — but more flexible and suited for JSON documents.

---
## Why Use Aggregations?

✅ Transform and reshape documents.
✅ Group and summarize data (e.g., counts, averages).
✅ Filter and sort documents.
✅ Perform joins (`$lookup`) and windowed calculations.
✅ Avoid transferring unnecessary data to your application.

---
## Basic Syntax

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$customerId", totalSpent: { $sum: "$amount" } } },
  { $sort: { totalSpent: -1 } }
]);
```

Here’s what’s happening:
1️⃣ `$match`: filter documents (like `WHERE`).
2️⃣ `$group`: group by a field and compute aggregates (like `GROUP BY`).
3️⃣ `$sort`: order the results.

---
## Common Pipeline Stages

### `$match`

Filter documents by criteria (similar to `.find()`):

```js
{ $match: { age: { $gte: 18 } } }
```

### `$group`

Group documents and calculate aggregates:

```js
{ 
  $group: {
    _id: "$department",
    totalSalary: { $sum: "$salary" },
    avgSalary: { $avg: "$salary" }
  }
}
```

### `$sort`

Sort the output:

```js
{ $sort: { createdAt: -1 } }
```

### `$project`

Shape or transform the output documents:

```js
{ $project: { name: 1, total: { $multiply: ["$price", "$quantity"] } } }
```

### `$limit` / `$skip`

Paginate results:

```js
{ $skip: 20 }, { $limit: 10 }
```

### `$lookup`

Perform a join with another collection:

```js
{
  $lookup: {
    from: "products",
    localField: "productId",
    foreignField: "_id",
    as: "productInfo"
  }
}
```

### `$unwind`

Break apart arrays into separate documents:

```js
{ $unwind: "$tags" }
```

---

## Notes on Performance

* Use `$match` as early as possible to reduce the dataset.
* Indexes can improve the first `$match` stage, but aggregation pipelines often work in-memory.
* Avoid pipelines that output very large intermediate results — these can hit memory limits.

---
## Debugging & Analyzing

Use `.explain()` to inspect how MongoDB executes your aggregation:

```js
db.orders.aggregate([...]).explain("executionStats");
```

---

The **Aggregation Framework** is one of MongoDB’s most powerful tools — allowing you to replace complex application logic with efficient database-side computation.

---

#mongodb