# Binary Search

## Description
Binary Search is an efficient algorithm for finding an item from a sorted list of items. It works by repeatedly dividing in half the portion of the list that could contain the item, until you've narrowed down the possible locations to just one.

## When to Use
- When searching for an element in a **sorted** array or list.
- For implementing lookup functions in databases or dictionaries where data is ordered.
- As a subroutine in more complex algorithms that require efficient searching.

## Time Complexity
- **Worst-case:** O(log n)
- **Average-case:** O(log n)
- **Best-case:** O(1)

## Space Complexity
- O(1) (iterative)
- O(log n) (recursive, due to recursion stack)

## Implementations

### JavaScript
```javascript
function binarySearch(arr, target) {
    let low = 0;
    let high = arr.length - 1;

    while (low <= high) {
        let mid = Math.floor((low + high) / 2);

        if (arr[mid] === target) {
            return mid; // Target found at index mid
        } else if (arr[mid] < target) {
            low = mid + 1; // Search in the right half
        } else {
            high = mid - 1; // Search in the left half
        }
    }
    return -1; // Target not found
}

let arrJS = [1, 5, 7, 8, 9, 10, 12, 15, 20];
console.log("Binary Search (JS) - target 9 at index:", binarySearch(arrJS, 9)); // 4
console.log("Binary Search (JS) - target 100 at index:", binarySearch(arrJS, 100)); // -1
```

### TypeScript
```typescript
function binarySearchTS<T>(arr: T[], target: T): number {
    let low = 0;
    let high = arr.length - 1;

    while (low <= high) {
        let mid = Math.floor((low + high) / 2);

        if (arr[mid] === target) {
            return mid; // Target found at index mid
        } else if (arr[mid] < target) {
            low = mid + 1; // Search in the right half
        } else {
            high = mid - 1; // Search in the left half
        }
    }
    return -1; // Target not found
}

let arrTS: number[] = [1, 5, 7, 8, 9, 10, 12, 15, 20];
console.log("Binary Search (TS) - target 9 at index:", binarySearchTS(arrTS, 9)); // 4
console.log("Binary Search (TS) - target 100 at index:", binarySearchTS(arrTS, 100)); // -1
```

### Python
```python
def binary_search(arr, target):
    low = 0
    high = len(arr) - 1

    while low <= high:
        mid = (low + high) // 2

        if arr[mid] == target:
            return mid # Target found at index mid
        elif arr[mid] < target:
            low = mid + 1 # Search in the right half
        else:
            high = mid - 1 # Search in the left half
    return -1 # Target not found

arr_py = [1, 5, 7, 8, 9, 10, 12, 15, 20]
print("Binary Search (Python) - target 9 at index:", binary_search(arr_py, 9)) # 4
print("Binary Search (Python) - target 100 at index:", binary_search(arr_py, 100)) # -1
```

### PHP
```php
<?php

function binarySearchPHP($arr, $target) {
    $low = 0;
    $high = count($arr) - 1;

    while ($low <= $high) {
        $mid = floor(($low + $high) / 2);

        if ($arr[$mid] === $target) {
            return $mid; // Target found at index mid
        } else if ($arr[$mid] < $target) {
            $low = $mid + 1; // Search in the right half
        } else {
            $high = $mid - 1; // Search in the left half
        }
    }
    return -1; // Target not found
}

$arrPHP = [1, 5, 7, 8, 9, 10, 12, 15, 20];
echo "Binary Search (PHP) - target 9 at index: " . binarySearchPHP($arrPHP, 9) . "\n"; // 4
echo "Binary Search (PHP) - target 100 at index: " . binarySearchPHP($arrPHP, 100) . "\n"; // -1

?>
```