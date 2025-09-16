# PHP: Inheritance

Inheritance is a fundamental concept in Object-Oriented Programming (OOP) that allows a class to inherit properties and methods from another class. The class that inherits is called the **child class** (or subclass, derived class), and the class being inherited from is called the **parent class** (or superclass, base class).

## Why Use Inheritance?

*   **Code Reusability**: Common properties and methods can be defined once in a parent class and reused by multiple child classes.
*   **Extensibility**: Child classes can extend or override the behavior of the parent class without modifying the parent class itself.
*   **Logical Grouping**: Helps in organizing classes into a hierarchical structure that models real-world relationships (e.g., a `Car` is a `Vehicle`).

## Implementing Inheritance

In PHP, inheritance is achieved using the `extends` keyword.

```php
<?php
// Parent class
class Vehicle {
    public $brand;

    public function __construct($brand) {
        $this->brand = $brand;
    }

    public function startEngine() {
        return "The " . $this->brand . " vehicle engine has started.";
    }

    public function stopEngine() {
        return "The " . $this->brand . " vehicle engine has stopped.";
    }
}

// Child class Car inherits from Vehicle
class Car extends Vehicle {
    public $model;

    public function __construct($brand, $model) {
        // Call the parent constructor to initialize parent properties
        parent::__construct($brand);
        $this->model = $model;
    }

    public function drive() {
        return "The " . $this->brand . " " . $this->model . " is driving.";
    }
}

// Child class Motorcycle inherits from Vehicle
class Motorcycle extends Vehicle {
    public $type;

    public function __construct($brand, $type) {
        parent::__construct($brand);
        $this->type = $type;
    }

    public function ride() {
        return "The " . $this->brand . " " . $this->type . " is being ridden.";
    }
}

$myCar = new Car("Toyota", "Camry");
echo $myCar->startEngine(); // Inherited method
echo $myCar->drive();       // Car's own method

$myMotorcycle = new Motorcycle("Harley-Davidson", "Cruiser");
echo $myMotorcycle->startEngine(); // Inherited method
echo $myMotorcycle->ride();        // Motorcycle's own method
?>
```

## The `parent::` Keyword

When a child class has its own constructor or method with the same name as a parent method, you can call the parent's version using `parent::`.

```php
<?php
class ParentClass {
    public function sayHello() {
        echo "Hello from ParentClass!\n";
    }
}

class ChildClass extends ParentClass {
    public function sayHello() {
        parent::sayHello(); // Call the parent's sayHello method
        echo "Hello from ChildClass!\n";
    }

    public function __construct() {
        // If ParentClass had a constructor, you'd call parent::__construct() here
        echo "ChildClass object created.\n";
    }
}

$obj = new ChildClass();
$obj->sayHello();
/* Outputs:
ChildClass object created.
Hello from ParentClass!
Hello from ChildClass!
*/
?>
```

## Method Overriding

Method overriding occurs when a child class provides a specific implementation for a method that is already defined in its parent class. This allows child classes to have their own unique behavior for inherited methods.

```php
<?php
class Animal {
    public function makeSound() {
        return "Generic animal sound.\n";
    }
}

class Dog extends Animal {
    // Overriding the makeSound method
    public function makeSound() {
        return "Woof! Woof!\n";
    }
}

class Cat extends Animal {
    // Overriding the makeSound method
    public function makeSound() {
        return "Meow! Meow!\n";
    }
}

$animal = new Animal();
echo $animal->makeSound(); // Outputs: Generic animal sound.

$dog = new Dog();
echo $dog->makeSound();     // Outputs: Woof! Woof!

$cat = new Cat();
echo $cat->makeSound();     // Outputs: Meow! Meow!
?>
```

## The `final` Keyword

The `final` keyword can be used to prevent a class from being inherited or to prevent a method from being overridden.

```php
<?php
final class BaseClass {
    // This class cannot be extended
}

// class MyClass extends BaseClass {} // This would cause a fatal error

class AnotherBase {
    final public function cannotOverride() {
        return "This method cannot be overridden.\n";
    }
}

class DerivedClass extends AnotherBase {
    // public function cannotOverride() {} // This would cause a fatal error
}
?>
```

## Access Modifiers and Inheritance

*   `public`: Inherited and accessible.
*   `protected`: Inherited and accessible within the child class, but not from outside the child class instance.
*   `private`: Not inherited. A child class cannot access private members of its parent.

```php
<?php
class ParentExample {
    public $publicProp = "Public";
    protected $protectedProp = "Protected";
    private $privateProp = "Private";

    public function getProtected() {
        return $this->protectedProp;
    }

    public function getPrivate() {
        // return $this->privateProp; // This would work within ParentExample
        return "Cannot access private from outside class.";
    }
}

class ChildExample extends ParentExample {
    public function accessParentProps() {
        echo $this->publicProp . "\n";    // Works
        echo $this->protectedProp . "\n"; // Works (within child class)
        // echo $this->privateProp; // Error: Undefined property (not inherited)
    }
}

$obj = new ChildExample();
$obj->accessParentProps();
// echo $obj->protectedProp; // Error: Cannot access protected property from outside
?>
```