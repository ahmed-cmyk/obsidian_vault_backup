# PHP: Interfaces and Abstract Classes

Interfaces and abstract classes are crucial concepts in Object-Oriented Programming (OOP) that promote abstraction and polymorphism. They define contracts or blueprints for classes, ensuring that certain methods are implemented by concrete classes.

## 1. Abstract Classes

An abstract class is a class that cannot be instantiated on its own. It serves as a base class for other classes to inherit from. Abstract classes can have both abstract methods (methods declared but not implemented) and concrete methods (methods with implementation).

### Key Characteristics of Abstract Classes:

*   Declared with the `abstract` keyword.
*   Cannot be instantiated directly (`new AbstractClass()` will cause an error).
*   Can contain abstract methods, which must be implemented by non-abstract child classes.
*   Can contain non-abstract (concrete) methods with full implementations.
*   Can have properties (variables).
*   A child class that inherits from an abstract class must implement all of its abstract methods, or it must also be declared abstract.

### Example of Abstract Class:

```php
<?php
abstract class Vehicle {
    protected $brand;

    public function __construct($brand) {
        $this->brand = $brand;
    }

    // Abstract method: child classes must implement this
    abstract public function drive();

    // Concrete method: has an implementation
    public function getBrand() {
        return $this->brand;
    }

    public function startEngine() {
        return "Engine started for " . $this->brand . ".\n";
    }
}

class Car extends Vehicle {
    public function drive() {
        return "Driving the car: " . $this->brand . ".\n";
    }
}

class Motorcycle extends Vehicle {
    public function drive() {
        return "Riding the motorcycle: " . $this->brand . ".\n";
    }
}

// $vehicle = new Vehicle("Generic"); // Fatal error: Cannot instantiate abstract class Vehicle

$car = new Car("Toyota");
echo $car->startEngine(); // Outputs: Engine started for Toyota.
echo $car->drive();       // Outputs: Driving the car: Toyota.

$motorcycle = new Motorcycle("Harley");
echo $motorcycle->startEngine(); // Outputs: Engine started for Harley.
echo $motorcycle->drive();       // Outputs: Riding the motorcycle: Harley.
?>
```

## 2. Interfaces

An interface defines a contract that classes must adhere to. It specifies a set of methods that a class implementing the interface must implement. Interfaces contain only method declarations (no implementation) and constants.

### Key Characteristics of Interfaces:

*   Declared with the `interface` keyword.
*   Cannot have properties (variables).
*   All methods declared in an interface must be `public`.
*   Methods in an interface cannot have a body (implementation).
*   A class can implement multiple interfaces using the `implements` keyword.
*   Interfaces can extend other interfaces.

### Example of Interface:

```php
<?php
interface Logger {
    public function logMessage($message);
    public function logError($error);
}

class FileLogger implements Logger {
    public function logMessage($message) {
        echo "Logging message to file: " . $message . "\n";
        // Logic to write to a file
    }

    public function logError($error) {
        echo "Logging error to file: " . $error . "\n";
        // Logic to write error to a file
    }
}

class DatabaseLogger implements Logger {
    public function logMessage($message) {
        echo "Logging message to database: " . $message . "\n";
        // Logic to write to a database
    }

    public function logError($error) {
        echo "Logging error to database: " . $error . "\n";
        // Logic to write error to a database
    }
}

$fileLogger = new FileLogger();
$fileLogger->logMessage("User logged in.");
$fileLogger->logError("Failed to connect to API.");

$dbLogger = new DatabaseLogger();
$dbLogger->logMessage("Data saved successfully.");
$dbLogger->logError("Database connection lost.");

// Polymorphism in action: a function expecting a Logger interface
function processLog(Logger $logger, $msg, $err) {
    $logger->logMessage($msg);
    $logger->logError($err);
}

processLog($fileLogger, "Another message", "Another error");
?>
```

## Abstract Classes vs. Interfaces: When to Use Which?

| Feature             | Abstract Class                               | Interface                                    |
| :------------------ | :------------------------------------------- | :------------------------------------------- |
| **Type of Members** | Can have abstract and concrete methods, properties, constants. | Can only have method declarations (public) and constants. No properties. |
| **Instantiation**   | Cannot be instantiated directly.             | Cannot be instantiated directly.             |
| **Inheritance**     | A class can `extend` only one abstract class. | A class can `implement` multiple interfaces. |
| **Purpose**         | Defines a common base for related classes that share some implementation. | Defines a contract for unrelated classes to adhere to. |
| **Constructor**     | Can have a constructor.                      | Cannot have a constructor.                   |

### When to use Abstract Classes:

*   When you want to provide a common base class with some default implementation and some methods that must be implemented by child classes.
*   When you have a strong "is-a" relationship (e.g., `Car` IS-A `Vehicle`).
*   When you need to define non-public members (protected/private).

### When to use Interfaces:

*   When you want to define a contract for what a class must do, regardless of its inheritance hierarchy.
*   When you want to achieve polymorphism across different class hierarchies.
*   When a class needs to implement multiple behaviors (PHP does not support multiple inheritance of classes, but it does support multiple interface implementation).
*   When you want to decouple components and allow for flexible dependency injection.

In many cases, you might use both together: an abstract class providing common functionality, and interfaces defining specific behaviors that classes can opt into.