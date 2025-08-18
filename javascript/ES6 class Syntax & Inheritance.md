# ES6 `class` Syntax & Inheritance

ES6 introduced `class` syntax as syntactic sugar over the prototype system.

---
## Defining a Class
- Define a constructor and instance methods inside the `class` body.

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a sound.`);
    }
}

const dog = new Animal("Buddy");
dog.speak(); // Buddy makes a sound.
```

---
## Class Inheritance

- Use extends to create a subclass.
- Use super() to call the parent constructor.
- Override parent methods if desired.

```js
class Dog extends Animal {
    constructor(name, breed) {
        super(name); 
        this.breed = breed;
    }

    speak() {
        console.log(`${this.name} barks!`);
    }

    fetch() {
        console.log(`${this.name} fetches the ball.`);
    }
}

const poodle = new Dog("Max", "Poodle");
poodle.speak(); // Max barks!
poodle.fetch(); // Max fetches the ball.
Behind the scenes:

Dog.prototype.__proto__ === Animal.prototype
```

---

#javascript