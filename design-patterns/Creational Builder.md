# Creational: Builder

**Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

---
## Structure

### `Product`

The `Product` is the complex object that’s being built. It consists of multiple parts that need to be assembled step-by-step.

### `Builder`

The `Builder` interface specifies methods for creating the different parts of the Product objects.

### `Concrete Builders`

`Concrete Builders` provide specific implementations of the building steps. Each builder is capable of building a variation of the product.

In addition to building product parts, concrete builders often implement a `getResult()` method that returns the final product.

### `Director`

The `Director` class defines the order in which to execute the building steps. It works with a builder object through the builder interface. The client assigns a specific builder to the director, and then the director uses it to construct the product.

The `Director` is optional. You can construct products directly with the builder if you need more control over the building process.

### `Client`

The `Client` creates both the builder and the director. It then initiates the construction process and retrieves the product from the builder.

---
## Applicability

* Use the Builder pattern when you want to construct complex objects step by step.
* Use it when you need to create different representations of the same product.
* Apply Builder to isolate complex construction code from the actual business logic and product classes.

---
## How to Implement

1. Analyze the target product to see if it can be broken into smaller parts or steps.
2. Create the `Product` class and define all possible configuration fields or components.
3. Create the `Builder` interface and declare methods for constructing the different parts of the product.
4. Implement one or more `ConcreteBuilder` classes for different representations or configurations of the product.
5. Optionally, create a `Director` class to encapsulate a standard construction sequence.
6. In client code:
   * Instantiate the builder and pass it to the director (if used).
   * Trigger the construction process.
   * Retrieve the result from the builder using a `getResult()` or equivalent method.

---
## Example

Let’s say you want to construct a `House` object step by step. A house typically has several parts: foundation, structure, roof, and interior. Using the **Builder Pattern**, we separate the construction logic from the final object.

### `Product`

The `House` is the complex object being built.

```js
class House {
  foundation?: string;
  structure?: string;
  roof?: string;
  interior?: string;

  describe() {
    console.log(`House with ${this.foundation}, ${this.structure}, ${this.roof}, and ${this.interior}.`);
  }
}
```

---
### `Builder Interface`

Defines the building steps — one for each part of the `House`.

```js
interface HouseBuilder {
  buildFoundation(): this;
  buildStructure(): this;
  buildRoof(): this;
  buildInterior(): this;
  getResult(): House;
}
```

---
### `Concrete Builder`

Implements the building steps and internally manages the `House` object.

```js
class ConcreteHouseBuilder implements HouseBuilder {
  private house = new House();

  buildFoundation() {
    this.house.foundation = "Concrete Slab";
    return this;
  }

  buildStructure() {
    this.house.structure = "Wood and Brick";
    return this;
  }

  buildRoof() {
    this.house.roof = "Composite Shingles";
    return this;
  }

  buildInterior() {
    this.house.interior = "Drywall and Paint";
    return this;
  }

  getResult(): House {
    return this.house;
  }
}
```

---

### `Director` (optional)

Encapsulates standard construction sequences (basic house, luxury house, etc.)

```js
class Director {
  constructor(private builder: HouseBuilder) {}

  constructBasicHouse() {
    this.builder.buildFoundation().buildStructure().buildRoof();
  }

  constructLuxuryHouse() {
    this.builder
      .buildFoundation()
      .buildStructure()
      .buildRoof()
      .buildInterior();
  }
}
```

---

### `Client`

* Creates the builder and optionally a director.
* Initiates the construction process.
* Gets the final result from the builder.

```js
const builder = new ConcreteHouseBuilder();
const director = new Director(builder);

director.constructLuxuryHouse();
const house = builder.getResult();
house.describe(); // House with Concrete Slab, Wood and Brick, Composite Shingles, and Drywall and Paint.
```

### Key Notes

* Builders allow **step-by-step construction** with varying configurations.
* The same building process can create **different representations** of the product.
* The **director is optional** — useful when construction steps are fixed or reused.

---
## Pros and Cons

| Pros | Cons |
|---|---|
| You can construct objects step-by-step, decouple the construction process from the final representation. | The overall design may become more complex if the product has a simple structure. |
| You can reuse the same construction code for different representations. | Requires creating multiple classes for builders, which may increase code size. |
| Follows the **Single Responsibility Principle** by separating construction logic from the actual product. |  | 
| Gives finer control over the construction process. |  |

---

#design-patterns