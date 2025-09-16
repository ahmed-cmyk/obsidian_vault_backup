# Bubble Sort

## Description
Bubble Sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, which indicates that the list is sorted.

## When to Use
- Primarily for educational purposes to understand sorting algorithms.
- For very small datasets where simplicity of implementation outweighs efficiency.
- Not recommended for large datasets due to its poor time complexity.

## Time Complexity
- **Worst-case:** O(n^2)
- **Average-case:** O(n^2)
- **Best-case:** O(n) (when the array is already sorted)

## Space Complexity
- O(1) (in-place sorting)

## Implementations

### JavaScript
```javascript
function bubbleSort(arr) {
    let n = arr.length;
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

let arrJS = [64, 34, 25, 12, 22, 11, 90];
console.log("Sorted array (JS):", bubbleSort(arrJS)); // [11, 12, 22, 25, 34, 64, 90]
```

### TypeScript
```typescript
function bubbleSortTS<T>(arr: T[]): T[] {
    let n = arr.length;
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

let arrTS: number[] = [64, 34, 25, 12, 22, 11, 90];
console.log("Sorted array (TS):", bubbleSortTS(arrTS)); // [11, 12, 22, 25, 34, 64, 90]
```

### Python
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j] # Swap
    return arr

arr_py = [64, 34, 25, 12, 22, 11, 90]
print("Sorted array (Python):", bubble_sort(arr_py)) # [11, 12, 22, 25, 34, 64, 90]
```

### PHP
```php
<?php

function bubbleSortPHP($arr) {
    $n = count($arr);
    for ($i = 0; $i < $n - 1; $i++) {
        for ($j = 0; $j < $n - $i - 1; $j++) {
            if ($arr[$j] > $arr[$j + 1]) {
                // Swap $arr[$j] and $arr[$j+1]
                $temp = $arr[$j];
                $arr[$j] = $arr[$j + 1];
                $arr[$j + 1] = $temp;
            }
        }
    }
    return $arr;
}

$arrPHP = [64, 34, 25, 12, 22, 11, 90];
echo "Sorted array (PHP): ";
print_r(bubbleSortPHP($arrPHP)); // Array ( [0] => 11 [1] => 12 [2] => 22 [3] => 25 [4] => 34 [5] => 64 [6] => 90 )

?>
```