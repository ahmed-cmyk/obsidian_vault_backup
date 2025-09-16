# PHP: Operators

Operators are used to perform operations on variables and values. PHP supports a wide range of operators.

## 1. Arithmetic Operators

Used for common mathematical operations.

| Operator | Name           | Example        | Result |
| :------- | :------------- | :------------- | :----- |
| `+`      | Addition       | `$x + $y`      | Sum    |
| `-`      | Subtraction    | `$x - $y`      | Difference |
| `*`      | Multiplication | `$x * $y`      | Product |
| `/`      | Division       | `$x / $y`      | Quotient |
| `%`      | Modulus        | `$x % $y`      | Remainder |
| `**`     | Exponentiation | `$x ** $y`     | `$x` to the power of `$y` |

```php
<?php
$a = 10;
$b = 3;
echo $a + $b; // 13
echo $a % $b; // 1
?>
```

## 2. Assignment Operators

Used to assign values to variables.

| Operator | Example      | Same As      |
| :------- | :----------- | :----------- |
| `=`      | `$x = $y`    | `$x = $y`    |
| `+=`     | `$x += $y`   | `$x = $x + $y` |
| `-=`     | `$x -= $y`   | `$x = $x - $y` |
| `*=`     | `$x *= $y`   | `$x = $x * $y` |
| `/=`     | `$x /= $y`   | `$x = $x / $y` |\n| `%=`     | `$x %= $y`   | `$x = $x % $y` |
| `**=`    | `$x **= $y`  | `$x = $x ** $y` |

```php
<?php
$x = 10;
$x += 5; // $x is now 15
?>
```

## 3. Comparison Operators

Used to compare two values (returns `true` or `false`).

| Operator | Name                 | Example        | Result |
| :------- | :------------------- | :------------- | :----- |
| `==`     | Equal                | `$x == $y`     | True if `$x` is equal to `$y` (value only) |
| `===`    | Identical            | `$x === $y`    | True if `$x` is equal to `$y`, and they are of the same type |
| `!=`     | Not equal            | `$x != $y`     | True if `$x` is not equal to `$y` |
| `<>`     | Not equal            | `$x <> $y`     | True if `$x` is not equal to `$y` |
| `!==`    | Not identical        | `$x !== $y`    | True if `$x` is not equal to `$y`, or they are not of the same type |
| `>`      | Greater than         | `$x > $y`      | True if `$x` is greater than `$y` |
| `<`      | Less than            | `$x < $y`      | True if `$x` is less than `$y` |
| `>=`     | Greater than or equal to | `$x >= $y`     | True if `$x` is greater than or equal to `$y` |
| `<=`     | Less than or equal to | `$x <= $y`     | True if `$x` is less than or equal to `$y` |
| `<=>`    | Spaceship            | `$x <=> $y`    | Returns -1 if `$x < $y`, 0 if `$x == $y`, or 1 if `$x > $y` |

```php
<?php
$a = 5;
$b = "5";
echo $a == $b;  // true (value is same)
echo $a === $b; // false (type is different)
?>
```

## 4. Logical Operators

Used to combine conditional statements.

| Operator | Name | Example        | Result |
| :------- | :--- | :------------- | :----- |
| `and`    | And  | `$x and $y`    | True if both `$x` and `$y` are true |
| `or`     | Or   | `$x or $y`     | True if either `$x` or `$y` is true |
| `xor`    | Xor  | `$x xor $y`    | True if either `$x` or `$y` is true, but not both |
| `&&`     | And  | `$x && $y`     | True if both `$x` and `$y` are true |
| `||`     | Or   | `$x || $y`     | True if either `$x` or `$y` is true |
| `!`      | Not  | `!$x`          | True if `$x` is not true |

```php
<?php
$age = 20;
$has_license = true;

if ($age >= 18 && $has_license) {
    echo "Eligible to drive.";
}
?>
```

## 5. String Operators

Used for string manipulation.

| Operator | Name        | Example        | Result |
| :------- | :---------- | :------------- | :----- |
| `.`      | Concatenation | `$txt1 . $txt2` | Concatenation of `$txt1` and `$txt2` |
| `.`      | Concatenation assignment | `$txt1 .= $txt2` | Appends `$txt2` to `$txt1` |

```php
<?php
$greeting = "Hello";
$name = "World";
echo $greeting . " " . $name; // Outputs: Hello World

$message = "PHP";
$message .= " is fun!"; // $message is now "PHP is fun!"
?>
```

## 6. Increment/Decrement Operators

Used to increment or decrement a variable's value.

| Operator | Name             | Example        | Result |
| :------- | :--------------- | :------------- | :----- |
| `++$x`   | Pre-increment    | Increments `$x` by one, then returns `$x` |
| `$x++`   | Post-increment   | Returns `$x`, then increments `$x` by one |
| `--$x`   | Pre-decrement    | Decrements `$x` by one, then returns `$x` |
| `$x--`   | Post-decrement   | Returns `$x`, then decrements `$x` by one |

```php
<?php
$i = 5;
echo ++$i; // Outputs 6, then $i is 6
echo $i++; // Outputs 6, then $i is 7
?>
```

## 7. Array Operators

Used to compare arrays.

| Operator | Name      | Example        | Result |
| :------- | :-------- | :------------- | :----- |
| `+`      | Union     | `$x + $y`      | Union of `$x` and `$y` |
| `==`     | Equality  | `$x == $y`     | True if `$x` and `$y` have the same key/value pairs |
| `===`    | Identity  | `$x === $y`    | True if `$x` and `$y` have the same key/value pairs in the same order and of the same types |
| `!=`     | Inequality | `$x != $y`     | True if `$x` is not equal to `$y` |
| `<>`     | Inequality | `$x <> $y`     | True if `$x` is not equal to `$y` |
| `!==`    | Non-identity | `$x !== $y`    | True if `$x` is not identical to `$y` |

```php
<?php
$arr1 = ["a" => 1, "b" => 2];
$arr2 = ["c" => 3, "a" => 1];
$arr3 = ["a" => 1, "b" => 2];

print_r($arr1 + $arr2); // Union: combines unique keys
var_dump($arr1 == $arr3); // true
var_dump($arr1 === $arr3); // true
?>
```

## 8. Conditional Assignment Operators

| Operator | Name        | Example        | Result |
| :------- | :---------- | :------------- | :----- |
| `?:`     | Ternary     | `$x ? $y : $z` | Returns `$y` if `$x` is true, otherwise `$z` |
| `??`     | Null coalescing | `$x ?? $y`     | Returns `$x` if `$x` exists and is not `NULL`, otherwise `$y` |

```php
<?php
// Ternary
$age = 20;
$status = ($age >= 18) ? "Adult" : "Minor";
echo $status; // Outputs: Adult

// Null coalescing
$name = $_GET['user'] ?? "Guest";
echo $name; // Outputs "Guest" if $_GET['user'] is not set or is null
?>
```