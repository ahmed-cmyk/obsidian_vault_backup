# PHP: Functions

Functions are blocks of code designed to perform a specific task. They help organize code, make it reusable, and improve readability. PHP has a vast number of built-in functions, and you can also define your own (user-defined functions).

## Defining a Function

A user-defined function declaration starts with the keyword `function`:

```php
<?php
function functionName() {
    // code to be executed
    echo "Hello from a function!";
}

// Calling the function
functionName(); // Outputs: Hello from a function!
?>
```

## Function Arguments

Information can be passed to functions through arguments. Arguments are like variables. They are specified after the function name, inside the parentheses.

```php
<?php
function greet($name) {
    echo "Hello, " . $name . "!\n";
}

greet("Alice"); // Outputs: Hello, Alice!
greet("Bob");   // Outputs: Hello, Bob!
?>
```

### Default Argument Values

You can set a default value for an argument. If the function is called without that argument, the default value will be used.

```php
<?php
function setHeight($minheight = 50) {
    echo "The height is : " . $minheight . "\n";
}

setHeight(350); // Outputs: The height is : 350
setHeight();    // Uses the default value (50). Outputs: The height is : 50
?>
```

### Variable-length Argument Lists

PHP supports functions that accept a variable number of arguments using `...` (splat operator).

```php
<?php
function sum(...$numbers) {
    $total = 0;
    foreach ($numbers as $n) {
        $total += $n;
    }
    return $total;
}

echo sum(1, 2, 3);     // Outputs: 6
echo sum(10, 20, 30, 40); // Outputs: 100
?>
```

## Returning Values

To return a value from a function, use the `return` statement.

```php
<?php
function addNumbers($num1, $num2) {
    $sum = $num1 + $num2;
    return $sum;
}

$result = addNumbers(5, 3);
echo $result; // Outputs: 8
?>
```

## Type Declarations

PHP 7 introduced type declarations for function arguments and return values. This helps with code clarity and error detection.

```php
<?php
function add(int $a, int $b): int {
    return $a + $b;
}

// This will work
echo add(5, 10); // Outputs: 15

// This might throw a TypeError depending on strict_types setting
// echo add(5, "10 days");
?>
```

To enforce strict typing, you can add `declare(strict_types=1);` at the very top of your PHP file.

## Scope of Variables

Variables declared inside a function have local scope and cannot be accessed outside the function. Variables declared outside a function have global scope.

```php
<?php
$x = 10; // Global scope

function myTest() {
    // echo $x; // This would cause an error (Undefined variable $x)
    $y = 5; // Local scope
    echo $y;
}

myTest(); // Outputs: 5
// echo $y; // This would cause an error (Undefined variable $y)
?>
```

### `global` Keyword

To access a global variable from within a function, use the `global` keyword.

```php
<?php
$x = 5;
$y = 10;

function mySum() {
    global $x, $y;
    $y = $x + $y;
}

mySum();
echo $y; // Outputs: 15
?>
```

### `$GLOBALS` Array

Alternatively, you can use the `$GLOBALS` superglobal array to access global variables.

```php
<?php
$x = 5;
$y = 10;

function mySum2() {
    $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
}

mySum2();
echo $y; // Outputs: 15
?>
```

### `static` Keyword

Normally, when a function is completed, all of its variables are deleted. However, sometimes you want a local variable not to be deleted. For this, use the `static` keyword.

```php
<?php
function myCounter() {
    static $count = 0;
    $count++;
    echo $count . "\n";
}

myCounter(); // Outputs: 1
myCounter(); // Outputs: 2
myCounter(); // Outputs: 3
?>
```