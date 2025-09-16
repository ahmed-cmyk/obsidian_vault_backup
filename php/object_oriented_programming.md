# PHP: Object-Oriented Programming (OOP) Introduction

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects", which can contain data and code: data in the form of fields (often known as attributes or properties), and code in the form of procedures (often known as methods). PHP has had robust OOP features since PHP 5.

## Why OOP?

OOP offers several benefits for software development:

*   **Modularity**: Objects create self-contained modules, making code easier to understand, maintain, and debug.
*   **Reusability**: Classes can be reused in different parts of the application or in different projects, saving development time.
*   **Flexibility/Extensibility**: New features can be added easily without affecting existing code.
*   **Maintainability**: Changes in one part of the system are less likely to affect other parts.
*   **Data Hiding/Security**: Encapsulation allows controlling access to data, protecting it from unintended external modification.

## Core Concepts of OOP

There are four main pillars of Object-Oriented Programming:

### 1. Encapsulation

Encapsulation is the bundling of data (attributes) and methods (functions) that operate on the data into a single unit, or class. It also involves restricting direct access to some of an object's components, which is a means of preventing accidental interference and misuse of the data.

*   **Access Modifiers**: PHP uses `public`, `protected`, and `private` keywords to control the visibility of properties and methods.
    *   `public`: Accessible from anywhere.
    *   `protected`: Accessible within the class itself and by inherited classes.
    *   `private`: Accessible only within the class that defines it.

```php
<?php
class Car {
    public $model; // Accessible from anywhere
    protected $color; // Accessible within Car and its subclasses
    private $engineNumber; // Accessible only within Car

    public function __construct($model, $color, $engineNumber) {
        $this->model = $model;
        $this->color = $color;
        $this->engineNumber = $engineNumber;
    }

    public function getModel() {
        return $this->model;
    }

    protected function getColor() {
        return $this->color;
    }

    private function getEngineNumber() {
        return $this->engineNumber;
    }
}

$myCar = new Car("Toyota", "Blue", "XYZ123");
echo $myCar->getModel(); // Works
// echo $myCar->color; // Error: Cannot access protected property
// echo $myCar->engineNumber; // Error: Cannot access private property
?>
```

### 2. Inheritance

Inheritance is a mechanism where a new class (subclass or derived class) is created from an existing class (superclass or base class). The subclass inherits properties and methods from the superclass, promoting code reusability.

*   The `extends` keyword is used to inherit from a class.

```php
<?php
class Vehicle {
    public $brand;

    public function __construct($brand) {
        $this->brand = $brand;
    }

    public function start() {
        return "The " . $this->brand . " vehicle is starting.";
    }
}

class Car extends Vehicle {
    public function drive() {
        return "The " . $this->brand . " car is driving.";
    }
}

$myCar = new Car("BMW");
echo $myCar->start(); // Inherited method
echo $myCar->drive(); // Car's own method
?>
```

### 3. Polymorphism

Polymorphism means "many forms". In OOP, it refers to the ability of objects of different classes to respond to the same method call in their own specific ways. This is often achieved through method overriding (in inheritance) or interfaces.

```php
<?php
interface Shape {
    public function calculateArea();
}

class Circle implements Shape {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }

    public function calculateArea() {
        return M_PI * $this->radius * $this->radius;
    }
}

class Rectangle implements Shape {
    public $width;
    public $height;

    public function __construct($width, $height) {
        $this->width = $width;
        $this->height = $height;
    }

    public function calculateArea() {
        return $this->width * $this->height;
    }
}

$circle = new Circle(5);
$rectangle = new Rectangle(4, 6);

function printArea(Shape $shape) {
    echo "Area: " . $shape->calculateArea() . "\n";
}

printArea($circle);    // Outputs: Area: 78.539816339745
printArea($rectangle); // Outputs: Area: 24
?>
```

### 4. Abstraction

Abstraction is the concept of hiding the complex implementation details and showing only the essential features of the object. In PHP, abstraction is achieved using abstract classes and interfaces.

*   **Abstract Classes**: Cannot be instantiated directly and may contain abstract methods (methods declared but not implemented).
*   **Interfaces**: Define a contract for classes to implement, specifying methods that must be implemented by any class that implements the interface.

```php
<?php
abstract class Animal {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }

    abstract public function makeSound(); // Abstract method

    public function eat() {
        return $this->name . " is eating.";
    }
}

class Dog extends Animal {
    public function makeSound() {
        return "Woof!";
    }
}

class Cat extends Animal {
    public function makeSound() {
        return "Meow!";
    }
}

$dog = new Dog("Buddy");
echo $dog->makeSound(); // Outputs: Woof!
echo $dog->eat();     // Outputs: Buddy is eating.

$cat = new Cat("Whiskers");
echo $cat->makeSound(); // Outputs: Meow!
?>
```

These four pillars form the foundation of OOP in PHP, enabling developers to write more organized, reusable, and maintainable code.
