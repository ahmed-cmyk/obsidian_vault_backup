# PHP: Variables and Data Types

In PHP, variables are used to store information. They are denoted by a dollar sign (`$`) followed by the variable name. PHP is a loosely typed language, meaning you don't have to declare the data type of a variable, it is determined by the value assigned to it.

## Variable Declaration and Assignment

```php
<?php
$name = "John Doe"; // String
$age = 30;         // Integer
$price = 19.99;    // Float (or Double)
$is_admin = true;  // Boolean
$colors = array("red", "green", "blue"); // Array
$user = null;      // NULL

// You can also assign the result of an expression
$sum = $age + 5;

echo $name;  // Outputs: John Doe
echo $age;   // Outputs: 30
?>
```

## PHP Data Types

PHP supports several data types:

1.  **String**: A sequence of characters. Can be enclosed in single (`'`) or double (`"`) quotes.
    ```php
    $greeting = "Hello, World!";
    $single_quote = 'This is a string with single quotes.';
    ```

2.  **Integer**: Non-decimal numbers (e.g., 1, 100, -5).
    ```php
    $quantity = 10;
    $year = 2023;
    ```

3.  **Float (or Double)**: Numbers with a decimal point or in exponential form.
    ```php
    $temperature = 25.5;
    $pi = 3.14159;
    ```

4.  **Boolean**: Represents two possible states: `true` or `false`.
    ```php
    $is_active = true;
    $has_permission = false;
    ```

5.  **Array**: Stores multiple values in a single variable. Values can be of different data types.
    ```php
    $fruits = array("apple", "banana", "cherry");
    $person = ["name" => "Alice", "age" => 25]; // Short array syntax
    ```

6.  **Object**: An instance of a class. Used for storing data and information on how to process that data.
    ```php
    class Car {
        public $model;
        function __construct($model) {
            $this->model = $model;
        }
    }
    $myCar = new Car("Volvo");
    ```

7.  **NULL**: A variable that has no value assigned to it. It is the only possible value of the `null` type.
    ```php
    $empty_var = null;
    ```

8.  **Resource**: Special variables, holding references to external resources (e.g., database connections, file handles).
    ```php
    // Example: file handle resource
    $file = fopen("example.txt", "r");
    ```

## Type Juggling and Type Casting

PHP's loose typing allows for automatic type conversion (type juggling) in many contexts. You can also explicitly cast variables to a different type.

```php
<?php
$num_str = "10";
$num_int = 5;

$sum = $num_str + $num_int; // Type juggling: "10" becomes 10 (integer)
echo $sum; // Outputs: 15

$casted_int = (int) "20.5"; // Type casting: "20.5" becomes 20
echo $casted_int; // Outputs: 20

$casted_bool = (bool) ""; // Empty string becomes false
echo ($casted_bool ? "true" : "false"); // Outputs: false
?>
```
