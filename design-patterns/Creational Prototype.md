# Creational: Prototype

**Prototype** is a creational design pattern that lets you copy existing objects without making your code dependent on their concrete classes.

---
## Structure

### `Prototype Interface`

The `Prototype` interface declares a `clone()` method that must be implemented by all classes that support cloning. This method is used to create a new object that’s a copy of the current instance.

### `Concrete Prototypes`

`Concrete Prototypes` implement the `clone()` method. Each class defines how cloning should be handled, especially when dealing with complex or nested objects (deep vs shallow copy).

### `Client`

The `Client` can clone objects via the prototype interface without knowing their concrete classes. It can produce many identical or slightly modified copies of complex objects efficiently.

---
## Applicability

* Use the Prototype pattern when:
  * You need to create many objects that share the same structure or configuration.
  * The cost of creating a new object with `new` is more expensive than copying an existing one.
  * You want to avoid creating subclasses just to copy objects with specific values.
  * You want to decouple the client code from the specific classes of the objects it works with.

---
## How to Implement

1. Define a `Prototype` interface with a `clone()` method.
2. Make all product classes implement the `Prototype` interface and define how copying should be performed.
   * Use shallow or deep copying depending on the complexity of the object.
3. In the client code, store a registry of prototype objects that can be cloned as needed.
4. When you need a new object, call the `clone()` method on the prototype instead of instantiating a new one from scratch.

---
## Use Cases

The **Prototype Pattern** is most useful when creating a new object from scratch is **expensive**, **error-prone**, or requires **too many repetitive steps**. Instead of re-initializing everything manually, you clone an existing, pre-configured instance.

---
### 1. **Cloning Pre-Fetched Database Records**

In database-heavy applications, fetching and configuring entities from a database can be costly — especially when there are joins, computed properties, or lazy-loaded relations.

#### 🧠 Why clone?

* Once an object is fetched and populated from the DB (e.g. with related models, validations, transformations), it may be reused as a **template** for similar objects.
* Cloning allows you to reuse all the structure, relations, and formatting *without hitting the database again*.

#### 📍 Example Scenario:

Suppose you’re developing a CRM and you want to create a new `Customer` object that’s similar to a template customer profile you fetched from the database:

```ts
tsCopyEditconst baseCustomer = await db.getCustomerTemplate('default');

// Instead of rebuilding the whole structure:
const newCustomer = baseCustomer.clone();
newCustomer.name = "Ahmed Ikram";
newCustomer.id = generateUniqueId();
```

✅ Benefits:

* Avoids another database roundtrip
* Retains default relationships, settings, and preferences
* Improves performance in high-load scenarios

---

### 2. **Game Engines (Spawning Characters or Objects)**

Games often require rapid instantiation of similar, complex objects:

* 3D models with meshes, animations, sounds, scripts
* Enemies, NPCs, power-ups

Using the Prototype pattern:

```ts
tsCopyEditconst newEnemy = enemyPrototype.clone();
newEnemy.setPosition(x, y);
```

✅ No need to reconfigure all components and behaviors.

---
### 3. **Graphic Editors (Figma, Photoshop)**

User creates a complex shape or component with many layers, effects, and settings. Instead of constructing each copy from scratch:

```ts
tsCopyEditconst duplicate = originalShape.clone();
duplicate.moveTo(x, y);
```

✅ Preserves styles, constraints, layers, and metadata.

---
### 4. **Document or Report Templates**

Instead of regenerating the base structure of a document:

```ts
tsCopyEditconst invoice = invoiceTemplate.clone();
invoice.fillOutWithCustomerData(data);
```

✅ Faster, simpler, and less error-prone than using constructors or configuration chains.

---
### 5. **Avoiding Subclass Explosion**

Imagine you have 10+ product variations, each needing slight tweaks. Instead of subclassing for every combo, you can:

```ts
tsCopyEditconst baseProduct = productRegistry.get("DefaultProduct");
const customProduct = baseProduct.clone();
customProduct.price = 99.99;
```

✅ More flexible than subclassing or using Factory/Builder when the configuration is runtime-specific.

---
## Pros and Cons

| Pros | Cons |
|---|---|
| You can clone objects without coupling to their concrete classes. | Cloning complex objects with nested references may require deep copy logic. |
| Reduces overhead of expensive object creation. | Each class must implement its own `clone()` method, which may be error-prone. | 
| Great for cases where object configuration is costly or repetitive. | Cloning may break encapsulation by exposing internal object details. |
| Can simplify object creation in dynamic systems. |  |

---

#design-patterns