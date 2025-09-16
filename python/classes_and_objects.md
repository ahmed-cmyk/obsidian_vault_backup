# Python Classes and Objects

## Classes

A class is a blueprint for creating objects (a particular data structure), providing initial values for state (member variables or attributes), and implementations of behavior (member functions or methods).

```python
class MyClass:
    x = 5

p1 = MyClass()
print(p1.x)
```

## Objects

An object is an instance of a class. You can create many objects from one class.

### The `__init__()` Function

All classes have a function called `__init__()`, which is always executed when the class is being initiated. It is used to assign values to object properties, or other operations that are necessary to do when the object is being created.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

p1 = Person("John", 36)

print(p1.name)
print(p1.age)
```

### Object Methods

Objects can also contain methods. Methods in objects are functions that belong to the object.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def myfunc(self):
        print(f"Hello my name is {self.name}")

p1 = Person("John", 36)
p1.myfunc()
```

### The `self` Parameter

The `self` parameter is a reference to the current instance of the class, and is used to access variables that belongs to the class. It does not have to be named `self`, you can call it whatever you like, but it has to be the first parameter of any function in the class.

## Inheritance

Inheritance allows us to define a class that inherits all the methods and properties from another class.

- **Parent class**: The class being inherited from, also called base class.
- **Child class**: The class that inherits from another class, also called derived class.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError("Subclass must implement abstract method")

class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

dog = Dog("Buddy")
cat = Cat("Whiskers")

print(dog.speak())
print(cat.speak())
```