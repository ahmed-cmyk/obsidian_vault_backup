# Maps & Sets

ES6 introduced `Map` and `Set` as new built-in data structures, offering advantages over plain objects and arrays for specific use cases.

---
## Map vs Plain Objects:

A Map is a collection of key-value pairs where keys can be of any data type (objects, functions, primitives, NaN, null), and the insertion order of elements is preserved.

* **Key Types**: Keys can be anything (objects, functions, primitives).
* **Order**: Insertion order is guaranteed.
* **Size**: `map.size` property directly gives the number of key-value pairs.
* **Iteration**: Directly iterable with `for...of`.
* **Performance**: Generally better performance for frequent additions and deletions of key-value pairs.

```js
const myMap = new Map();
myMap.set('name', 'Alice');
myMap.set(1, 'number one');
myMap.set({}, 'an object as key');

console.log(myMap.get('name')); // Alice
console.log(myMap.size); // 3
```

**Plain Objects** (`{}`) are also collections of key-value pairs, but they have several limitations when used as maps:

* **Key Types**: Keys are implicitly converted to **strings** or Symbols. You cannot use objects or non-string primitives (other than Symbols) as direct keys.
* **Order**: Property order is not guaranteed (though modern engines generally preserve insertion order for non-integer keys).
* **Size**: No direct `size` property; requires `Object.keys().length`.
* **Iteration**: `for...in` iterates over enumerable string keys (including inherited); `Object.keys()`, `Object.values()`, `Object.entries()` are used for direct properties.
* **Performance**: Can have performance issues for very large numbers of frequently changing keys due to prototype chain lookups.

### When to use `Map`

* When you need to use non-string values (objects, functions, etc.) as keys.
* When the order of insertion matters.
* When you frequently add or remove key-value pairs.
* When you need to easily get the size of the collection.

### When to use Plain Objects

* When you need simple structured data (like a record or dictionary with known string keys).
* When you need to serialize data to JSON.
* When you rely on object methods or `this` binding.

---
## Set vs Arrays

A `Set` is a collection of unique values. It can store values of any data type, and duplicate values are automatically ignored.

* **Uniqueness**: Stores only unique values.
* **Order**: Insertion order is preserved.
* **Size**: `set.size` property directly gives the number of unique values.
* **Iteration**: Directly iterable with `for...of`.
* **Operations**: Efficient `add()`, `delete()`, `has()`.

```js
const mySet = new Set();
mySet.add(1);
mySet.add('hello');
mySet.add(1); // Duplicate, ignored
mySet.add({ a: 1 }); // Unique object

console.log(mySet.size); // 3
console.log(mySet.has('hello')); // true
```

**Arrays** (`[]`) are ordered collections that **can contain duplicate values**.

* **Uniqueness**: Allows duplicate values.
* **Order**: Elements are ordered by index.
* **Size**: `array.length` property.
* **Operations**: `push()`, `pop()`, `splice()`, `indexOf()`, etc. Checking for existence (`includes()`) or removing duplicates can be less efficient than `Set` for large collections.

### When to use `Set`

* When you need to store a collection of unique items.
* When you need to quickly check for the presence of an item.
* When you need to remove duplicates from an existing collection.

### When to use Arrays

* When the order of elements is important and you need to access them by index.
* When you need to store duplicate values.
* When you need to perform transformations like `map`, `filter`, `reduce`.

---
#javascript