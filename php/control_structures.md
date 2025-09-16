# PHP: Control Structures

Control structures are fundamental to programming, allowing you to control the flow of execution based on conditions or to repeat blocks of code. PHP provides several control structures.

## 1. Conditional Statements

### `if...else...elseif`

Executes different blocks of code based on whether a condition is true or false.

```php
<?php
$age = 18;

if ($age < 13) {
    echo "You are a child.";
} elseif ($age < 18) {
    echo "You are a teenager.";
} else {
    echo "You are an adult.";
}

// Shorthand for single statements (not recommended for readability)
if ($age >= 18) echo "Adult";
?>
```

### `switch`

Used to perform different actions based on different conditions on the same variable.

```php
<?php
$favcolor = "red";

switch ($favcolor) {
    case "red":
        echo "Your favorite color is red!";
        break;
    case "blue":
        echo "Your favorite color is blue!";
        break;
    case "green":
        echo "Your favorite color is green!";
        break;
    default:
        echo "Your favorite color is neither red, blue, nor green!";
}
?>
```

## 2. Looping Statements

Looping statements are used to execute the same block of code a specified number of times or while a certain condition is met.

### `while`

Executes a block of code as long as the specified condition is true.

```php
<?php
$i = 1;
while ($i <= 5) {
    echo "The number is: " . $i . "\n";
    $i++;
}
?>
```

### `do...while`

Executes a block of code once, and then repeats the loop as long as the specified condition is true. The condition is checked *after* the first execution.

```php
<?php
$i = 1;
do {
    echo "The number is: " . $i . "\n";
    $i++;
} while ($i <= 5);
?>
```

### `for`

Used when you know in advance how many times the script should run.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    echo "The number is: " . $i . "\n";
}
?>
```

### `foreach`

Used to loop through arrays.

```php
<?php
$colors = array("red", "green", "blue", "yellow");

foreach ($colors as $value) {
    echo $value . "\n";
}

// With key-value pairs for associative arrays
$age = array("Peter" => "35", "Ben" => "37", "Joe" => "43");
foreach ($age as $name => $val) {
    echo "Key: " . $name . ", Value: " . $val . "\n";
}
?>
```

## 3. Jump Statements

### `break`

Used to exit a loop or a `switch` statement.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    if ($i == 5) {
        break; // Exit the loop when $i is 5
    }
    echo "The number is: " . $i . "\n";
}
?>
```

### `continue`

Used to skip the current iteration of a loop and continue with the next iteration.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    if ($i % 2 == 0) {
        continue; // Skip even numbers
    }
    echo "The odd number is: " . $i . "\n";
}
?>
```

### `goto` (Discouraged)

Allows you to jump to another section in the program. Generally discouraged due to making code harder to read and maintain.

```php
<?php
goto a;
echo "Hello World!";

a: // This is the label
echo "Hello from label 'a'!";
?>
```
