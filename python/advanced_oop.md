# Python Advanced Object-Oriented Programming (OOP)

Building upon the basics of classes and objects, Python offers several advanced OOP concepts that enable more robust, flexible, and maintainable code. These include deeper dives into inheritance, polymorphism, abstraction, encapsulation, and special methods.

## 1. Inheritance (Deeper Dive)

Inheritance allows a class (child/derived class) to inherit attributes and methods from another class (parent/base class). This promotes code reusability and establishes an "is-a" relationship.

### Method Overriding

Child classes can provide their own implementation of a method that is already defined in their parent class. This is called method overriding.

```python
class Animal:
    def speak(self):
        print("Animal makes a sound")

class Dog(Animal):
    def speak(self):
        print("Woof!") # Overriding the speak method

class Cat(Animal):
    def speak(self):
        print("Meow!") # Overriding the speak method

animal = Animal()
dog = Dog()
cat = Cat()

animal.speak() # Output: Animal makes a sound
dog.speak()    # Output: Woof!
cat.speak()    # Output: Meow!
```

### `super()` Function

The `super()` function allows a child class to call methods from its parent class. This is particularly useful when you want to extend the parent's method rather than completely replace it.

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand

    def display_info(self):
        print(f"Brand: {self.brand}")

class Car(Vehicle):
    def __init__(self, brand, model):
        super().__init__(brand) # Call parent's __init__
        self.model = model

    def display_info(self):
        super().display_info() # Call parent's display_info
        print(f"Model: {self.model}")

my_car = Car("Toyota", "Camry")
my_car.display_info()
# Output:
# Brand: Toyota
# Model: Camry
```

### Multiple Inheritance

Python supports multiple inheritance, where a class can inherit from multiple base classes. The order of inheritance matters due to Method Resolution Order (MRO).

```python
class A:
    def method(self):
        print("Method from A")

class B:
    def method(self):
        print("Method from B")

class C(A, B):
    pass

class D(B, A):
    pass

c_obj = C()
c_obj.method() # Output: Method from A (A is listed first in C(A, B))

d_obj = D()
d_obj.method() # Output: Method from B (B is listed first in D(B, A))

# You can inspect MRO using .mro() or help()
print(C.mro())
# Output: [<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>]
```

## 2. Polymorphism

Polymorphism means "many forms." In OOP, it refers to the ability of different objects to respond to the same method call in their own way. This is often achieved through inheritance and method overriding.

```python
class Bird:
    def fly(self):
        print("Bird flies")

class Airplane:
    def fly(self):
        print("Airplane flies with engines")

class Superman:
    def fly(self):
        print("Superman flies without wings")

def make_it_fly(entity):
    entity.fly()

make_it_fly(Bird())      # Output: Bird flies
make_it_fly(Airplane())  # Output: Airplane flies with engines
make_it_fly(Superman())  # Output: Superman flies without wings
```

## 3. Abstraction

Abstraction means hiding the complex implementation details and showing only the essential features of an object. In Python, abstraction can be achieved using abstract classes and methods from the `abc` (Abstract Base Classes) module.

An abstract class cannot be instantiated, and abstract methods must be implemented by concrete (non-abstract) subclasses.

```python
from abc import ABC, abstractmethod

class Shape(ABC): # Inherit from ABC to make it an abstract base class
    @abstractmethod
    def area(self):
        pass # Abstract method, must be implemented by subclasses

    @abstractmethod
    def perimeter(self):
        pass # Abstract method

    def describe(self):
        print("This is a shape.")

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius ** 2

    def perimeter(self):
        return 2 * 3.14 * self.radius

# shape = Shape() # This would raise a TypeError: Can't instantiate abstract class Shape

circle = Circle(5)
print(f"Circle area: {circle.area()}")
print(f"Circle perimeter: {circle.perimeter()}")
circle.describe()
```

## 4. Encapsulation

Encapsulation is the bundling of data (attributes) and methods that operate on the data within a single unit (class), and restricting direct access to some of the object's components. This is done to prevent accidental modification of data.

Python doesn't have strict access modifiers like `public`, `private`, `protected`. Instead, it uses conventions:

-   **Public:** Attributes/methods without a leading underscore. Accessible from anywhere.
-   **Protected:** Attributes/methods starting with a single underscore (`_`). Conventionally, these are intended for internal use within the class or its subclasses, but they are still accessible from outside.
-   **Private:** Attributes/methods starting with double underscores (`__`). Python performs name mangling on these, making them harder (but not impossible) to access from outside the class. They are primarily meant to avoid naming conflicts in inheritance.

```python
class MyClass:
    def __init__(self):
        self.public_var = "I am public"
        self._protected_var = "I am protected"
        self.__private_var = "I am private"

    def public_method(self):
        print("This is a public method.")

    def _protected_method(self):
        print("This is a protected method.")

    def __private_method(self):
        print("This is a private method.")

    def access_private(self):
        print(self.__private_var)
        self.__private_method()

obj = MyClass()
print(obj.public_var)
obj.public_method()

print(obj._protected_var) # Accessible, but convention says don't directly access
obj._protected_method()

# print(obj.__private_var) # AttributeError: 'MyClass' object has no attribute '__private_var'
# obj.__private_method() # AttributeError

obj.access_private() # Accessing private members via a public method

# Name mangling allows access like this (but don't do it!)
print(obj._MyClass__private_var)
```

## 5. Special Methods (Dunder Methods)

Special methods (also known as "magic methods" or "dunder methods" because they start and end with double underscores) allow you to define how objects of your class behave with built-in operations and functions (e.g., arithmetic operations, string representation, iteration).

| Method Name      | Description                                                              | Example Usage                               |
| :--------------- | :----------------------------------------------------------------------- | :------------------------------------------ |
| `__init__(self, ...)` | Constructor: Initializes a new instance of the class.                    | `obj = MyClass(...)`                        |
| `__str__(self)`  | String representation for human readability (called by `str()`, `print()`). | `print(obj)`                                |
| `__repr__(self)` | Official string representation for developers (called by `repr()`).      | `repr(obj)`                                 |
| `__len__(self)`  | Defines the behavior for the `len()` function.                           | `len(obj)`                                  |
| `__add__(self, other)` | Defines behavior for the `+` operator.                                   | `obj1 + obj2`                               |
| `__getitem__(self, key)` | Defines behavior for accessing items using `[]` (e.g., `obj[key]`).      | `obj[0]`, `obj['name']`                     |
| `__setitem__(self, key, value)` | Defines behavior for assigning items using `[]` (e.g., `obj[key] = value`). | `obj[0] = 10`                               |
| `__delitem__(self, key)` | Defines behavior for deleting items using `del` (e.g., `del obj[key]`).  | `del obj[0]`                                |
| `__call__(self, ...)` | Makes an instance of the class callable like a function.                 | `obj()`                                     |
| `__eq__(self, other)` | Defines behavior for the `==` operator.                                  | `obj1 == obj2`                              |
| `__lt__(self, other)` | Defines behavior for the `<` operator.                                   | `obj1 < obj2`                               |

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Point({self.x}, {self.y})"

    def __repr__(self):
        return f"Point(x={self.x}, y={self.y})"

    def __add__(self, other):
        if isinstance(other, Point):
            return Point(self.x + other.x, self.y + other.y)
        raise TypeError("Can only add Point objects")

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False

p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = Point(1, 2)

print(p1)      # Calls __str__ Output: Point(1, 2)
print(repr(p1)) # Calls __repr__ Output: Point(x=1, y=2)

p_sum = p1 + p2
print(p_sum)   # Calls __add__ Output: Point(4, 6)

print(p1 == p3) # Calls __eq__ Output: True
print(p1 == p2) # Calls __eq__ Output: False
```

## 6. Class Methods, Static Methods, and Instance Methods

-   **Instance Methods:** The most common type. They take `self` as the first argument and can access/modify instance-specific data.
-   **Class Methods:** Decorated with `@classmethod`. They take `cls` (the class itself) as the first argument. They can access/modify class-level data and are often used for alternative constructors.
-   **Static Methods:** Decorated with `@staticmethod`. They don't take `self` or `cls`. They are like regular functions but are logically part of the class and don't operate on instance or class-specific data.

```python
class MyClass:
    class_variable = 0

    def __init__(self, instance_variable):
        self.instance_variable = instance_variable

    def instance_method(self):
        print(f"Instance method: {self.instance_variable}")
        print(f"Accessing class variable from instance: {self.class_variable}")

    @classmethod
    def class_method(cls):
        print(f"Class method: Accessing class variable: {cls.class_variable}")
        cls.class_variable += 1

    @staticmethod
    def static_method(x, y):
        print(f"Static method: {x} + {y} = {x + y}")

obj1 = MyClass(10)
obj2 = MyClass(20)

obj1.instance_method()
obj2.instance_method()

MyClass.class_method() # Call via class
obj1.class_method()    # Can also call via instance, but cls will still be MyClass
print(MyClass.class_variable)

MyClass.static_method(5, 3)
obj1.static_method(1, 1)
```
