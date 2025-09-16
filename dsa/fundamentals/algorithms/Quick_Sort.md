# Quick Sort

## Description
Quick Sort is an efficient, in-place, comparison-based sorting algorithm. It works by selecting a 'pivot' element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively. This is an example of a divide and conquer algorithm.

## When to Use
- When average-case performance is critical, as it's typically faster than other O(n log n) algorithms in practice.
- When in-place sorting is preferred due to memory constraints.
- For large datasets where its efficiency shines.
- Not suitable for datasets where stability is required.

## Time Complexity
- **Worst-case:** O(n^2) (occurs with poor pivot selection, e.g., already sorted array)
- **Average-case:** O(n log n)
- **Best-case:** O(n log n)

## Space Complexity
- O(log n) (due to recursion stack, average case)
- O(n) (due to recursion stack, worst case)

## Implementations

### JavaScript
```javascript
function quickSort(arr, low, high) {
    if (low < high) {
        let pi = partition(arr, low, high);

        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
    return arr;
}

function partition(arr, low, high) {
    let pivot = arr[high];
    let i = (low - 1);

    for (let j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            [arr[i], arr[j]] = [arr[j], arr[i]]; // Swap
        }
    }
    [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]]; // Swap
    return (i + 1);
}

let arrJS = [10, 7, 8, 9, 1, 5];
let nJS = arrJS.length;
console.log("Sorted array (JS):", quickSort(arrJS, 0, nJS - 1)); // [1, 5, 7, 8, 9, 10]
```

### TypeScript
```typescript
function quickSortTS<T>(arr: T[], low: number, high: number): T[] {
    if (low < high) {
        let pi = partitionTS(arr, low, high);

        quickSortTS(arr, low, pi - 1);
        quickSortTS(arr, pi + 1, high);
    }
    return arr;
}

function partitionTS<T>(arr: T[], low: number, high: number): number {
    let pivot = arr[high];
    let i = (low - 1);

    for (let j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            [arr[i], arr[j]] = [arr[j], arr[i]]; // Swap
        }
    }
    [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]]; // Swap
    return (i + 1);
}

let arrTS: number[] = [10, 7, 8, 9, 1, 5];
let nTS = arrTS.length;
console.log("Sorted array (TS):", quickSortTS(arrTS, 0, nTS - 1)); // [1, 5, 7, 8, 9, 10]
```

### Python
```python
def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)

        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
    return arr

def partition(arr, low, high):
    pivot = arr[high]
    i = (low - 1)

    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i] # Swap

    arr[i + 1], arr[high] = arr[high], arr[i + 1] # Swap
    return (i + 1)

arr_py = [10, 7, 8, 9, 1, 5]
n_py = len(arr_py)
print("Sorted array (Python):", quick_sort(arr_py, 0, n_py - 1)) # [1, 5, 7, 8, 9, 10]
```

### PHP
```php
<?php

function quickSortPHP(&$arr, $low, $high) {
    if ($low < $high) {
        $pi = partitionPHP($arr, $low, $high);

        quickSortPHP($arr, $low, $pi - 1);
        quickSortPHP($arr, $pi + 1, $high);
    }
    return $arr;
}

function partitionPHP(&$arr, $low, $high) {
    $pivot = $arr[$high];
    $i = ($low - 1);

    for ($j = $low; $j <= $high - 1; $j++) {
        if ($arr[$j] < $pivot) {
            $i++;
            // Swap
            $temp = $arr[$i];
            $arr[$i] = $arr[$j];
            $arr[$j] = $temp;
        }
    }
    // Swap pivot to its correct position
    $temp = $arr[$i + 1];
    $arr[$i + 1] = $arr[$high];
    $arr[$high] = $temp;

    return ($i + 1);
}

$arrPHP = [10, 7, 8, 9, 1, 5];
$nPHP = count($arrPHP);
echo "Sorted array (PHP): ";
print_r(quickSortPHP($arrPHP, 0, $nPHP - 1)); // Array ( [0] => 1 [1] => 5 [2] => 7 [3] => 8 [4] => 9 [5] => 10 )

?>
```