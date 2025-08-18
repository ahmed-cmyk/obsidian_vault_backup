# MongoDB: Core Concepts & CRUD Operations

MongoDB is a **NoSQL, document-oriented database** that stores data in flexible, JSON-like documents (`BSON`).

It is schema-less by default and provides powerful querying and indexing capabilities.

---

## Core Concepts

### Document

The smallest unit of data in MongoDB, similar to a row in SQL.
A document is an **ordered set of key-value pairs**, stored as BSON.

```json
{
  _id: ObjectId("60df1..."),
  name: "John",
  age: 30,
  isActive: true
}
```

### Collection

A **group of documents**.
Similar to a table in SQL. Documents in a collection can have different “shapes.”

```js
db.users.find()
```

### Database

A **logical container** for collections. MongoDB can host many databases.

### `_id`

Every document has a unique `_id` field as its primary key, which MongoDB generates if not provided.

---

## CRUD Operations

### Create

Insert a single document:

```js
db.users.insertOne({ name: "Alice", age: 25 });
```

Insert multiple documents:

```js
db.users.insertMany([
  { name: "Bob", age: 30 },
  { name: "Carol", age: 28 }
]);
```

---

### Read

Find all documents:

```js
db.users.find()
```

Find documents with a filter:

```js
db.users.find({ age: { $gt: 25 } });
```

Find one document:

```js
db.users.findOne({ name: "Bob" });
```

Projection (only return certain fields):

```js
db.users.find({}, { name: 1, age: 1, _id: 0 });
```

---

### Update

Update a single document:

```js
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
);
```

Update multiple documents:

```js
db.users.updateMany(
  { isActive: false },
  { $set: { isActive: true } }
);
```

Replace a document:

```js
db.users.replaceOne(
  { name: "Carol" },
  { name: "Carol", age: 29, isActive: true }
);
```

---

### Delete

Delete a single document:

```js
db.users.deleteOne({ name: "Bob" });
```

Delete multiple documents:

```js
db.users.deleteMany({ isActive: false });
```

---

## Notes

* MongoDB is **schema-less**, but you can enforce schemas with validation rules or tools like Mongoose.
* Collections and databases are created implicitly when you insert a document.
* Use **indexes** to improve query performance.
* MongoDB supports **replication & sharding** for scalability and high availability.

---

#mongodb
