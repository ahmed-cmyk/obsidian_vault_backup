# PHP: Classes and Objects

In Object-Oriented Programming (OOP), a **class** is a blueprint or a template for creating objects, providing initial values for state (member variables) and implementations of behavior (member functions or methods). An **object** is an instance of a class.

## Defining a Class

A class is defined using the `class` keyword, followed by the class name and a pair of curly braces (`{}`).

```php
<?php
class Car {
    // Properties (variables) - define the state of an object
    public $brand;
    public $model;
    public $color;

    // Methods (functions) - define the behavior of an object
    public function startEngine() {
        return "The " . $this->brand . " " . $this->model . " engine has started.";
    }

    public function drive() {
        return "The " . $this->brand . " " . $this->model . " is driving.";
    }
}
?>
```

### Properties

Properties are variables that belong to a class. They define the attributes or characteristics of an object. They are declared with an access modifier (`public`, `protected`, or `private`).

### Methods

Methods are functions that belong to a class. They define the actions or behaviors that an object can perform.

## Creating Objects (Instantiation)

An object is created from a class using the `new` keyword.

```php
<?php
// Create an object of the Car class
$myCar = new Car();

// Accessing properties and assigning values
$myCar->brand = "Toyota";
$myCar->model = "Camry";
$myCar->color = "Red";

// Accessing methods
echo $myCar->startEngine(); // Outputs: The Toyota Camry engine has started.
echo $myCar->drive();       // Outputs: The Toyota Camry is driving.

// Create another object
$anotherCar = new Car();
$anotherCar->brand = "Honda";
$anotherCar->model = "Civic";
$anotherCar->color = "Blue";
echo $anotherCar->startEngine(); // Outputs: The Honda Civic engine has started.
?>
```

## The `__construct()` Method (Constructor)

The `__construct()` method is a special method that is automatically called when a new object is created. It's commonly used to initialize object properties.

```php
<?php
class Car {
    public $brand;
    public $model;
    public $color;

    // Constructor method
    public function __construct($brand, $model, $color) {
        $this->brand = $brand;
        $this->model = $model;
        $this->color = $color;
    }

    public function getDetails() {
        return "This is a " . $this->color . " " . $this->brand . " " . $this->model . ".";
    }
}

// Create objects, passing arguments to the constructor
$car1 = new Car("Toyota", "Camry", "Red");
echo $car1->getDetails(); // Outputs: This is a Red Toyota Camry.

$car2 = new Car("Honda", "Civic", "Blue");
echo $car2->getDetails(); // Outputs: This is a Blue Honda Civic.
?>
```

## The `__destruct()` Method (Destructor)

The `__destruct()` method is automatically called when the object is destroyed or the script is stopped. It can be used to perform cleanup tasks.

```php
<?php
class Car {
    public $brand;

    public function __construct($brand) {
        $this->brand = $brand;
        echo "A new car ( " . $this->brand . " ) is created.\n";
    }

    public function __destruct() {
        echo "The car ( " . $this->brand . " ) is destroyed.\n";
    }
}

$car = new Car("BMW");
echo "Script continues...\n";
// When the script finishes, __destruct() will be called automatically
?>
```

## The `$this` Keyword

`$this` refers to the current object. It is used to access properties and methods of the object within the class itself.

```php
<?php
class Person {
    public $name;

    public function setName($name) {
        $this->name = $name; // Assigns the passed $name to the object's name property
    }

    public function getName() {
        return $this->name;
    }
}

$person = new Person();
$person->setName("Alice");
echo $person->getName(); // Outputs: Alice
?>
```

## Access Modifiers (Visibility)

Properties and methods can have access modifiers to control their visibility:

*   `public`: The property or method can be accessed from anywhere.
*   `protected`: The property or method can be accessed within the class and by classes derived from that class.
*   `private`: The property or method can ONLY be accessed within the class that defines it.

```php
<?php
class BankAccount {
    public $accountNumber;
    protected $balance; // Accessible by BankAccount and its children
    private $pin;       // Accessible only by BankAccount

    public function __construct($accNum, $initialBalance, $pin) {
        $this->accountNumber = $accNum;
        $this->balance = $initialBalance;
        $this->pin = $pin;
    }

    public function deposit($amount) {
        if ($amount > 0) {
            $this->balance += $amount;
            return "Deposited $amount. New balance: " . $this->balance;
        }
        return "Deposit amount must be positive.";
    }

    protected function getPin() {
        return $this->pin;
    }

    // A public method to expose balance safely
    public function getBalance() {
        return $this->balance;
    }
}

$account = new BankAccount("12345", 1000, "0000");
echo $account->deposit(500); // Outputs: Deposited 500. New balance: 1500
// echo $account->balance; // Error: Cannot access protected property
// echo $account->pin;     // Error: Cannot access private property
?>
```

## Class Constants

Constants can be defined inside a class using the `const` keyword. They are case-sensitive and can be accessed without creating an object of the class.

```php
<?php
class Greeting {
    const MESSAGE = "Hello World!";

    public function sayHello() {
        echo self::MESSAGE; // Accessing constant within the class
    }
}

echo Greeting::MESSAGE; // Accessing constant outside the class

$greet = new Greeting();
$greet->sayHello();
?>
```

## Static Properties and Methods

Static properties and methods can be accessed without creating an instance of the class. They belong to the class itself, not to any specific object.

*   Use the `static` keyword.
*   Access with the `::` (Paamayim Nekudotayim, or scope resolution operator).

```php
<?php
class MathOperations {
    public static $pi = 3.14159;

    public static function add($a, $b) {
        return $a + $b;
    }

    public static function circleArea($radius) {
        return self::$pi * $radius * $radius; // Accessing static property within class
    }
}

// Accessing static property and method directly
echo MathOperations::$pi; // Outputs: 3.14159
echo MathOperations::add(10, 5); // Outputs: 15
echo MathOperations::circleArea(5); // Outputs: 78.53975
?>
```