# Hash Tables (Maps/Dictionaries)

## Description
A hash table (also known as a hash map, dictionary, or associative array) is a data structure that implements an associative array abstract data type, a structure that can map keys to values. A hash table uses a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

## When to Use
- When you need fast average-case time complexity for insertion, deletion, and lookup operations (O(1)).
- For implementing caches, databases, and symbol tables in compilers.
- When you need to store unique keys and their associated values.
- When the order of elements is not important.

## Time Complexity (Average Case)
- **Insertion:** O(1)
- **Deletion:** O(1)
- **Lookup:** O(1)

## Time Complexity (Worst Case)
- **Insertion:** O(n) (due to hash collisions)
- **Deletion:** O(n) (due to hash collisions)
- **Lookup:** O(n) (due to hash collisions)

## Space Complexity
- O(n) (where n is the number of key-value pairs)

## Implementations

### JavaScript (using `Map` object)
```javascript
// Using JavaScript's built-in Map object
let myMap = new Map();

// Insertion
myMap.set("name", "Alice");
myMap.set("age", 30);
myMap.set("city", "New York");

console.log(myMap); // Map(3) { 'name' => 'Alice', 'age' => 30, 'city' => 'New York' }

// Lookup
console.log(myMap.get("name")); // Alice
console.log(myMap.has("age")); // true

// Deletion
myMap.delete("city");
console.log(myMap); // Map(2) { 'name' => 'Alice', 'age' => 30 }

// Iteration
for (let [key, value] of myMap) {
    console.log(`${key}: ${value}`);
}
// Output:
// name: Alice
// age: 30
```

### TypeScript (using `Map` object)
```typescript
// Using TypeScript's built-in Map object
let myMap: Map<string, any> = new Map();

// Insertion
myMap.set("name", "Bob");
myMap.set("age", 25);
myMap.set("occupation", "Engineer");

console.log(myMap); // Map(3) { 'name' => 'Bob', 'age' => 25, 'occupation' => 'Engineer' }

// Lookup
console.log(myMap.get("name")); // Bob
console.log(myMap.has("occupation")); // true

// Deletion
myMap.delete("age");
console.log(myMap); // Map(2) { 'name' => 'Bob', 'occupation' => 'Engineer' }

// Iteration
for (let [key, value] of myMap) {
    console.log(`${key}: ${value}`);
}
// Output:
// name: Bob
// occupation: Engineer
```

### Python (using `dict`)
```python
# Using Python's built-in dictionary
my_dict = {}

# Insertion
my_dict["name"] = "Charlie"
my_dict["age"] = 35
my_dict["country"] = "Canada"

print(my_dict) # {'name': 'Charlie', 'age': 35, 'country': 'Canada'}

// Lookup
print(my_dict.get("name")) # Charlie
print("age" in my_dict) # True

// Deletion
del my_dict["country"]
print(my_dict) # {'name': 'Charlie', 'age': 35}

// Iteration
for key, value in my_dict.items():
    print(f"{key}: {value}")
// Output:
// name: Charlie
// age: 35
```

### PHP (using associative arrays)
```php
<?php

// Using PHP's associative arrays
$myArray = [];

// Insertion
$myArray["name"] = "David";
$myArray["age"] = 40;
$myArray["language"] = "PHP";

print_r($myArray);
/* Output:
Array
(
    [name] => David
    [age] => 40
    [language] => PHP
)
*/

// Lookup
echo $myArray["name"] . "\n"; // David
echo (array_key_exists("age", $myArray) ? "true" : "false") . "\n"; // true

// Deletion
unset($myArray["language"]);
print_r($myArray);
/* Output:
Array
(
    [name] => David
    [age] => 40
)
*/

// Iteration
foreach ($myArray as $key => $value) {
    echo "{$key}: {$value}\n";
}
/* Output:
name: David
age: 40
*/

?>
```