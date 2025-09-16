# Arrays

## Description
An array is a collection of items stored at contiguous memory locations. The idea is to store multiple items of the same type together. This makes it easier to calculate the position of each element by simply adding an offset to a base value.

## When to Use
- When you need to store a fixed-size collection of elements of the same type.
- When you need fast access to elements by index (O(1) time complexity).
- When memory efficiency is crucial, as arrays typically have less overhead than dynamic data structures.

## Implementations

### JavaScript
```javascript
// Declaration
let arr = [1, 2, 3, 4, 5];

// Accessing elements
console.log(arr[0]); // 1

// Modifying elements
arr[0] = 10;
console.log(arr); // [10, 2, 3, 4, 5]

// Adding elements (at the end)
arr.push(6);
console.log(arr); // [10, 2, 3, 4, 5, 6]

// Removing elements (from the end)
arr.pop();
console.log(arr); // [10, 2, 3, 4, 5]
```

### TypeScript
```typescript
// Declaration with type annotation
let arr: number[] = [1, 2, 3, 4, 5];

// Accessing elements
console.log(arr[0]); // 1

// Modifying elements
arr[0] = 10;
console.log(arr); // [10, 2, 3, 4, 5]

// Adding elements (at the end)
arr.push(6);
console.log(arr); // [10, 2, 3, 4, 5, 6]

// Removing elements (from the end)
arr.pop();
console.log(arr); // [10, 2, 3, 4, 5]
```

### Python
```python
# Declaration
arr = [1, 2, 3, 4, 5]

# Accessing elements
print(arr[0]) # 1

# Modifying elements
arr[0] = 10
print(arr) # [10, 2, 3, 4, 5]

# Adding elements (at the end)
arr.append(6)
print(arr) # [10, 2, 3, 4, 5, 6]

# Removing elements (from the end)
arr.pop()
print(arr) # [10, 2, 3, 4, 5]
```

### PHP
```php
<?php
// Declaration
$arr = [1, 2, 3, 4, 5];

// Accessing elements
echo $arr[0] . "\n"; // 1

// Modifying elements
$arr[0] = 10;
print_r($arr); // Array ( [0] => 10 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )

// Adding elements (at the end)
$arr[] = 6;
print_r($arr); // Array ( [0] => 10 [1] => 2 [2] => 3 [3] => 4 [4] => 5 [5] => 6 )

// Removing elements (from the end)
array_pop($arr);
print_r($arr); // Array ( [0] => 10 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )
?>
```