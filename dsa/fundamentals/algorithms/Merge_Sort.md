# Merge Sort

## Description
Merge Sort is an efficient, comparison-based, divide and conquer sorting algorithm. It divides the unsorted list into n sublists, each containing one element (a list of one element is considered sorted). Then, it repeatedly merges sublists to produce new sorted sublists until there is only one sorted list remaining.

## When to Use
- When stability is required (maintains the relative order of equal elements).
- When sorting linked lists, as it doesn't require random access to elements.
- When external sorting is needed (data too large to fit into memory).
- For parallel processing due to its divide and conquer nature.

## Time Complexity
- **Worst-case:** O(n log n)
- **Average-case:** O(n log n)
- **Best-case:** O(n log n)

## Space Complexity
- O(n) (due to the temporary arrays used during merging)

## Implementations

### JavaScript
```javascript
function mergeSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }

    const middle = Math.floor(arr.length / 2);
    const left = arr.slice(0, middle);
    const right = arr.slice(middle);

    return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right) {
    let result = [];
    let leftIndex = 0;
    let rightIndex = 0;

    while (leftIndex < left.length && rightIndex < right.length) {
        if (left[leftIndex] < right[rightIndex]) {
            result.push(left[leftIndex]);
            leftIndex++;
        } else {
            result.push(right[rightIndex]);
            rightIndex++;
        }
    }

    return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}

let arrJS = [38, 27, 43, 3, 9, 82, 10];
console.log("Sorted array (JS):", mergeSort(arrJS)); // [3, 9, 10, 27, 38, 43, 82]
```

### TypeScript
```typescript
function mergeSortTS<T>(arr: T[]): T[] {
    if (arr.length <= 1) {
        return arr;
    }

    const middle = Math.floor(arr.length / 2);
    const left = arr.slice(0, middle);
    const right = arr.slice(middle);

    return mergeTS(mergeSortTS(left), mergeSortTS(right));
}

function mergeTS<T>(left: T[], right: T[]): T[] {
    let result: T[] = [];
    let leftIndex = 0;
    let rightIndex = 0;

    while (leftIndex < left.length && rightIndex < right.length) {
        if (left[leftIndex] < right[rightIndex]) {
            result.push(left[leftIndex]);
            leftIndex++;
        } else {
            result.push(right[rightIndex]);
            rightIndex++;
        }
    }

    return result.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
}

let arrTS: number[] = [38, 27, 43, 3, 9, 82, 10];
console.log("Sorted array (TS):", mergeSortTS(arrTS)); // [3, 9, 10, 27, 38, 43, 82]
```

### Python
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = arr[:mid]
    right = arr[mid:]

    left = merge_sort(left)
    right = merge_sort(right)

    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])
    return result

arr_py = [38, 27, 43, 3, 9, 82, 10]
print("Sorted array (Python):", merge_sort(arr_py)) # [3, 9, 10, 27, 38, 43, 82]
```

### PHP
```php
<?php

function mergeSortPHP($arr) {
    $n = count($arr);
    if ($n <= 1) {
        return $arr;
    }

    $middle = floor($n / 2);
    $left = array_slice($arr, 0, $middle);
    $right = array_slice($arr, $middle);

    $left = mergeSortPHP($left);
    $right = mergeSortPHP($right);

    return mergePHP($left, $right);
}

function mergePHP($left, $right) {
    $result = [];
    $leftIndex = 0;
    $rightIndex = 0;

    while ($leftIndex < count($left) && $rightIndex < count($right)) {
        if ($left[$leftIndex] < $right[$rightIndex]) {
            $result[] = $left[$leftIndex];
            $leftIndex++;
        } else {
            $result[] = $right[$rightIndex];
            $rightIndex++;
        }
    }

    while ($leftIndex < count($left)) {
        $result[] = $left[$leftIndex];
        $leftIndex++;
    }

    while ($rightIndex < count($right)) {
        $result[] = $right[$rightIndex];
        $rightIndex++;
    }

    return $result;
}

$arrPHP = [38, 27, 43, 3, 9, 82, 10];
echo "Sorted array (PHP): ";
print_r(mergeSortPHP($arrPHP)); // Array ( [0] => 3 [1] => 9 [2] => 10 [3] => 27 [4] => 38 [5] => 43 [6] => 82 )

?>
```