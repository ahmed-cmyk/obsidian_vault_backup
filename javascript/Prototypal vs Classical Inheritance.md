# Prototypal vs Classical Inheritance

Comparison of JavaScript’s native model with traditional OOP inheritance.

---
## Prototypal Inheritance (JavaScript)
- **Objects inherit directly from other objects.**
- Flexible and dynamic.
- Prototype chain used for delegation.
- No rigid class hierarchy.
- Can modify prototypes at runtime.

```js
const animal = {
    speak() {
        console.log("Makes a sound.");
    }
};

const dog = Object.create(animal);
dog.speak(); // Makes a sound.
```

---
## Classical Inheritance (e.g., Java, C++)

- Classes inherit from classes, and objects are instances of classes.
- Rigid and static structure.
- Type hierarchies defined at compile time.
- Blueprint + instance model.

---

#javascript