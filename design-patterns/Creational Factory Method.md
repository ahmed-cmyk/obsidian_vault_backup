# Creational: Factory Method

**Factory Method** is a creational pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the types of objects that will be created.

---
## Structure

### `Product`

The `Product` declares the interface, which is common to all objects that can be produced by the creator and its subclasses.

### `Concrete Products`

`Concrete Products` are different implementations of the `Product` interface.

### `Creator`

The `Creator` class declares the factory method that returns new product objects. It’s important that the return type of this method matches the `Product` interface.

You can declare the factory method as `abstract` to force all subclasses to implement their own versions of the method. As an alternative, the base factory method can return some default `Product` type.

### `Concrete Creators`

`Concrete Creators` override the base factory method so it returns a different type of product.

> **NOTE**: The factory method doesn’t have to **create** new instances all the time. It can also return existing objects from a cache, an object pool, or an another source.

---
## Applicability

- Use the factory method when you don’t know beforehand the exact types of the dependencies that you’ll be working with.
- Use the Factory Method when you want to provide users of your library of framework with a way to extend its internal components.
- Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.

---
## Steps to Implement

1. **Define a Common Interface for Products**
- All product classes should implement a shared interface. This interface declares methods that are applicable to all products.

2. **Add a Factory Method to the Creator Class**
- Define an empty factory method inside the creator class. Its return type should be the product interface.

3. **Replace Direct Product Instantiations**
- Search the creator’s code for all instances where products are constructed using `new`. Replace those with calls to the factory method. Move the instantiation logic into the factory method.

4. **(Optional) Add a Control Parameter**
- If you need to vary which product gets created, consider adding a parameter to the factory method to help decide what to return.

5. **Factory Method Might Look Messy Initially**
- At this stage, the factory method may use a large `switch` or `if` chain. This is temporary and will be cleaned up in the next steps.

6. **Create Subclasses of the Creator**
- For each product type, create a subclass of the creator. Override the factory method to return the appropriate product. Extract the relevant code from the base factory method into the subclasses.

7. **Handle Overlapping Product Usage**
If one creator subclass uses multiple products (e.g., `GroundMail` might use both `Truck` and `Train`), consider:
* Using a control parameter in the factory method.
* Or, creating a separate subclass for the more specific case (e.g., `TrainMail`).

10. **Finalize the Base Factory Method**
* If the base factory method is now empty → make it `abstract`.
* If some default logic remains → keep it as a default implementation.

---
## Pros and Cons

| Pros | Cons |
|---|---|
| Avoids tight coupling between creators and concrete products | Introduces many new subclasses, which can make the class hierarchy deeper |
| **Single Responsibility Principle**: Product creation is centralized | May overcomplicate code if used prematurely or unnecessarily | 
| **Open/Closed Principle**: Add new products without changing client code | Refactoring existing code may require significant effort if product logic is scattered | 

---

#design-patterns