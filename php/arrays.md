# PHP: Arrays

An array is a special variable that can hold more than one value at a time. If you have a list of items (e.g., a list of car names, colors, etc.), storing the cars in single variables would look like this:

```php
$car1 = "Volvo";
$car2 = "BMW";
$car3 = "Toyota";
```

However, if you want to loop through the cars and find a specific one, and if you had not 3, but 300 cars, this would be a very inefficient way. The solution is an array.

## Types of Arrays

In PHP, there are three types of arrays:

1.  **Indexed arrays**: Arrays with a numeric index.
2.  **Associative arrays**: Arrays with named keys.
3.  **Multidimensional arrays**: Arrays containing one or more arrays.

## 1. Indexed Arrays

Indexed arrays can be created in two ways:

```php
<?php
// Method 1: Using array() function
$cars = array("Volvo", "BMW", "Toyota");

// Method 2: Using square brackets (short array syntax, PHP 5.4+)
$fruits = ["Apple", "Banana", "Cherry"];

// Accessing elements (indexes start from 0)
echo $cars[0];   // Outputs: Volvo
echo $fruits[1]; // Outputs: Banana

// Changing an element
$cars[1] = "Mercedes";
echo $cars[1]; // Outputs: Mercedes

// Adding an element (appends to the end)
$cars[] = "Audi";
print_r($cars); // Outputs: Array ( [0] => Volvo [1] => Mercedes [2] => Toyota [3] => Audi )
?>
```

### Looping Through an Indexed Array

```php
<?php
$cars = ["Volvo", "BMW", "Toyota"];
$arrlength = count($cars);

for ($x = 0; $x < $arrlength; $x++) {
    echo $cars[$x];
    echo "\n";
}
?>
```

## 2. Associative Arrays

Associative arrays use named keys that you assign to them.

```php
<?php
// Method 1: Using array() function
$age = array("Peter" => "35", "Ben" => "37", "Joe" => "43");

// Method 2: Using square brackets
$person = [
    "first_name" => "John",
    "last_name" => "Doe",
    "age" => 30
];

// Accessing elements
echo $age["Peter"]; // Outputs: 35
echo $person["last_name"]; // Outputs: Doe

// Changing an element
$age["Ben"] = "38";
echo $age["Ben"]; // Outputs: 38

// Adding an element
$age["Alice"] = "29";
print_r($age);
?>
```

### Looping Through an Associative Array

```php
<?php
$age = array("Peter" => "35", "Ben" => "37", "Joe" => "43");

foreach ($age as $x => $x_value) {
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "\n";
}
?>
```

## 3. Multidimensional Arrays

A multidimensional array is an array containing one or more arrays. PHP supports multidimensional arrays that are two, three, or more levels deep.

```php
<?php
$cars = [
    ["Volvo", 22, 18],
    ["BMW", 15, 13],
    ["Saab", 5, 2],
    ["Land Rover", 17, 15]
];

// Accessing elements
echo $cars[0][0]; // Outputs: Volvo
echo $cars[1][2]; // Outputs: 13

// Looping through a two-dimensional array
for ($row = 0; $row < 4; $row++) {
    echo "<p><b>Row number $row</b></p>";
    echo "<ul>";
    for ($col = 0; $col < 3; $col++) {
        echo "<li>" . $cars[$row][$col] . "</li>";
    }
    echo "</ul>";
}
?>
```

## Useful Array Functions

PHP provides a rich set of functions for working with arrays:

*   `count()`: Returns the number of elements in an array.
*   `sort()`: Sorts an indexed array in ascending order.
*   `rsort()`: Sorts an indexed array in descending order.
*   `asort()`: Sorts an associative array in ascending order, according to the value.
*   `ksort()`: Sorts an associative array in ascending order, according to the key.
*   `arsort()`: Sorts an associative array in descending order, according to the value.
*   `krsort()`: Sorts an associative array in descending order, according to the key.
*   `array_push()`: Pushes one or more elements onto the end of an array.
*   `array_pop()`: Pops the last element of the array off.
*   `array_merge()`: Merges one or more arrays.
*   `in_array()`: Checks if a value exists in an array.
*   `array_keys()`: Returns all the keys or a subset of the keys of an array.
*   `array_values()`: Returns all the values of an array.

```php
<?php
$numbers = [4, 2, 8, 1, 5];
sort($numbers);
print_r($numbers); // Outputs: Array ( [0] => 1 [1] => 2 [2] => 4 [3] => 5 [4] => 8 )

$fruits = ["Apple", "Banana"];
array_push($fruits, "Orange", "Grape");
print_r($fruits); // Outputs: Array ( [0] => Apple [1] => Banana [2] => Orange [3] => Grape )

if (in_array("Banana", $fruits)) {
    echo "Banana is in the array.\n";
}
?>
```