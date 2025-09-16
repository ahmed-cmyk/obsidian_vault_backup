# Python Descriptors

Descriptors are a powerful, yet often misunderstood, feature in Python that allow you to customize how attributes are accessed, assigned, or deleted on an object. They are objects that implement one or more of the descriptor protocol methods: `__get__`, `__set__`, and `__delete__`.

## What is a Descriptor?

A descriptor is an object that is assigned to a class attribute, and its methods (`__get__`, `__set__`, `__delete__`) are called whenever that attribute is accessed, assigned, or deleted on an *instance* of the class.

### Descriptor Protocol Methods:

-   `__get__(self, instance, owner)`:
    -   Called when the attribute is accessed (e.g., `obj.attribute`).
    -   `self`: The descriptor instance itself.
    -   `instance`: The instance of the class on which the attribute was accessed (e.g., `obj`). If accessed via the class (e.g., `MyClass.attribute`), `instance` is `None`.
    -   `owner`: The class that owns the descriptor (e.g., `MyClass`).
    -   Should return the attribute's value.

-   `__set__(self, instance, value)`:
    -   Called when the attribute is assigned (e.g., `obj.attribute = value`).
    -   `self`: The descriptor instance.
    -   `instance`: The instance of the class on which the attribute was assigned.
    -   `value`: The value being assigned to the attribute.

-   `__delete__(self, instance)`:
    -   Called when the attribute is deleted (e.g., `del obj.attribute`).
    -   `self`: The descriptor instance.
    -   `instance`: The instance of the class on which the attribute was deleted.

## Example: A Simple Descriptor for Validation

Let's create a descriptor that ensures an attribute is always a positive integer.

```python
class PositiveInteger:
    def __set_name__(self, owner, name):
        # This method is called when the descriptor is assigned to a class attribute.
        # It allows the descriptor to know the name of the attribute it's managing.
        self.public_name = name
        self.private_name = '_' + name # Store the actual value in a private attribute

    def __get__(self, instance, owner):
        if instance is None:
            return self # Accessing via class, return the descriptor itself
        return getattr(instance, self.private_name)

    def __set__(self, instance, value):
        if not isinstance(value, int):
            raise TypeError(f"{self.public_name} must be an integer")
        if value <= 0:
            raise ValueError(f"{self.public_name} must be a positive integer")
        setattr(instance, self.private_name, value)

    def __delete__(self, instance):
        delattr(instance, self.private_name)

class Product:
    price = PositiveInteger() # Assigning the descriptor to a class attribute
    quantity = PositiveInteger()

    def __init__(self, name, price, quantity):
        self.name = name
        self.price = price      # This calls PositiveInteger.__set__
        self.quantity = quantity # This calls PositiveInteger.__set__

# Usage
p = Product("Laptop", 1200, 5)
print(f"Product: {p.name}, Price: {p.price}, Quantity: {p.quantity}")

# Accessing via class
print(Product.price) # Returns the descriptor object itself

# Valid assignment
p.price = 1500
print(f"New price: {p.price}")

# Invalid assignments (will raise errors)
try:
    p.quantity = -2
except ValueError as e:
    print(e)

try:
    p.price = "abc"
except TypeError as e:
    print(e)

# Deleting an attribute
del p.price
try:
    print(p.price)
except AttributeError as e:
    print(e)
```

## Types of Descriptors

Descriptors can be classified based on the methods they implement:

1.  **Data Descriptors:** Implement both `__get__` and `__set__` (or `__delete__`). They take precedence over instance dictionaries.
2.  **Non-Data Descriptors:** Implement only `__get__`. They can be overridden by entries in the instance dictionary.

This distinction is important for understanding attribute lookup order.

## Attribute Lookup Order

When you access an attribute `obj.attr`, Python follows a specific order:

1.  **Check `obj.__class__.__dict__` for `attr`:**
    -   If `attr` is a data descriptor, its `__get__` method is called.
    -   If `attr` is not a descriptor, or is a non-data descriptor, Python proceeds to step 2.
2.  **Check `obj.__dict__` for `attr`:**
    -   If `attr` is found here, its value is returned.
3.  **Check `obj.__class__.__dict__` again for `attr`:**
    -   If `attr` is a non-data descriptor, its `__get__` method is called.
    -   If `attr` is a regular method or a non-descriptor, its value is returned.
4.  **Check base classes (MRO) for `attr`:** The process repeats for base classes.
5.  If not found, `AttributeError` is raised.

## Common Built-in Descriptors

Many built-in Python features are implemented using descriptors:

-   **Methods:** Functions defined in a class become non-data descriptors when accessed via an instance. They bind the instance to the function (creating a bound method).
-   **`@property`:** A built-in decorator that turns a method into a property, allowing you to define getters, setters, and deleters for attributes using a more Pythonic syntax. It's essentially a data descriptor.

    ```python
    class Circle:
        def __init__(self, radius):
            self._radius = radius # Store in a private-like attribute

        @property
        def radius(self):
            """The radius property."""
            return self._radius

        @radius.setter
        def radius(self, value):
            if not isinstance(value, (int, float)) or value <= 0:
                raise ValueError("Radius must be a positive number")
            self._radius = value

        @radius.deleter
        def radius(self):
            print("Deleting radius")
            del self._radius

    c = Circle(10)
    print(c.radius) # Calls the getter
    c.radius = 15   # Calls the setter
    print(c.radius)

    try:
        c.radius = -5
    except ValueError as e:
        print(e)

    del c.radius # Calls the deleter
    ```

-   **`@classmethod` and `@staticmethod`:** These decorators are also implemented as descriptors. They modify how methods are bound to the class or instance.

## When to Use Descriptors?

Descriptors are a low-level mechanism. You typically use them when:

-   You need to reuse attribute access logic across multiple classes or attributes.
-   You need fine-grained control over attribute access, validation, or transformation.
-   You are building a framework or library where you want to provide a custom attribute behavior (e.g., ORMs, validation libraries).

For simpler cases, `@property` is often a more readable and sufficient solution.
