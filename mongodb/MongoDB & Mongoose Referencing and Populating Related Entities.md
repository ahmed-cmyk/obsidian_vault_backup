# MongoDB & Mongoose: Referencing and Populating Related Entities

One of the most common tasks when working with MongoDB & Mongoose is retrieving documents **along with related data** — similar to joins in SQL.
In MongoDB, you usually model these relationships in one of two ways:

---
## 1️⃣ Embedding

You can embed one document inside another when the relationship is **one-to-few** and the embedded data is tightly coupled.

```js
const orderSchema = new mongoose.Schema({
  customerName: String,
  items: [
    {
      productName: String,
      quantity: Number
    }
  ]
});
```

This is good when:

* The embedded documents don’t grow unbounded.
* You always fetch the parent document with the child data.

---
## 2️⃣ Referencing (Normalization)

When the relationship is **one-to-many** or **many-to-many**, and the related documents are large, independent, or shared across many parents, you store references instead.

```js
const userSchema = new mongoose.Schema({
  name: String
});

const postSchema = new mongoose.Schema({
  title: String,
  content: String,
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
});
```

Here the `author` field stores the `_id` of a `User`.

---
## Populating References

To actually *fetch* the related data, you use Mongoose’s `.populate()` method.

```js
Post.find()
  .populate('author')
  .then(posts => {
    console.log(posts);
  });
```

The `author` field is automatically replaced with the full `User` document.

Output example:

```js
[
  {
    title: "Post 1",
    content: "Content of post",
    author: {
      _id: "65f7...",
      name: "Alice"
    }
  }
]
```

---
## Populating Multiple Levels

You can even populate nested references:

```js
Post.find()
  .populate({
    path: 'author',
    populate: { path: 'address' }
  });
```

---
## Tips

* Use `.select()` if you only need specific fields from the populated document:

```js
.populate('author', 'name email')
```

* Use `.lean()` if you don’t need full Mongoose document functionality, for better performance.

---
## When to Embed vs Reference?

| Embed | Reference |
|---|---|
| One-to-few | One-to-many |
| Always read together | Read independently |
| Size of child documents is small | Large/variable child documents |

---

#mongodb