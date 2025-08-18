# Iterables & Generators

Iterables** and **Generators** are core concepts in ES6+ that provide powerful ways to work with sequences of data, enabling more efficient and flexible iteration.

---
## Iterables

An iterable is any object that implements the Symbol.iterator method. This method must return an iterator object. An iterator is an object that has a next() method, which returns an object with two properties: value (the next item in the sequence) and done (a boolean indicating if the iteration is complete).

Many built-in JavaScript types are iterables, including `Array`, `String`, `Map`, `Set`, `NodeList`, etc.

```js
const myString = "hello";
const iterator = myString[Symbol.iterator]();

console.log(iterator.next()); // { value: 'h', done: false }
console.log(iterator.next()); // { value: 'e', done: false }
// ...
console.log(iterator.next()); // { value: undefined, done: true }

// Iterables can be used with:
for (const char of myString) { // for...of loop
    console.log(char);
}
const arr = [...myString]; // Spread operator
console.log(arr); // ['h', 'e', 'l', 'l', 'o']
```

---
## Generators

Generator functions are special functions that can be paused and resumed. They are defined using the function* syntax (note the asterisk) and use the yield keyword to pause execution and return a value. When a generator function is called, it doesn't execute its body immediately; instead, it returns a Generator object (which is itself an iterator and an iterable).

Each time the generator's `next()` method is called, it resumes execution from where it last `yield`ed, runs until the next `yield` statement, and returns the yielded value. When the generator function finishes (or encounters a `return`), `next()` returns `{ value: undefined, done: true }`.

```js
function* countGenerator() {
    yield 1;
    yield 2;
    yield 3;
    return "Finished counting"; // Value for the final 'done: true' state
}

const counter = countGenerator(); // Returns a generator object

console.log(counter.next()); // { value: 1, done: false }
console.log(counter.next()); // { value: 2, done: false }
console.log(counter.next()); // { value: 3, done: false }
console.log(counter.next()); // { value: 'Finished counting', done: true }
console.log(counter.next()); // { value: undefined, done: true }

// Generators are iterables, so they can be used with for...of
for (const num of countGenerator()) {
    console.log(num); // 1, 2, 3
}
```

### Use Cases for Generators

* **Lazy Evaluation**: Generating sequences of data on demand, rather than creating the entire sequence in memory at once (e.g., infinite sequences).
* **Asynchronous Programming**: Can be used with libraries like `co` or manually to manage asynchronous flows in a more sequential manner (though `async/await` is now the preferred method).
* **Implementing Custom Iterators**: A simpler way to make an object iterable.

---

#javascript