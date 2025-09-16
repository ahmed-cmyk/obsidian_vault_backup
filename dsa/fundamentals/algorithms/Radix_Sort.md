# Radix Sort

## Description
Radix Sort is a non-comparative integer sorting algorithm that sorts data with integer keys by grouping keys by individual digits which share the same significant position and value. It processes digits from least significant to most significant (LSD Radix Sort) or vice versa (MSD Radix Sort). It uses a stable sorting algorithm (like Counting Sort) as a subroutine to sort the numbers based on each digit.

## When to Use
- When sorting integers or strings of fixed length.
- When the range of values for each digit is small.
- Can be faster than comparison-based sorts (like Quick Sort, Merge Sort) for certain datasets, especially when the keys are large but the number of digits is small.
- Not suitable for floating-point numbers or general objects.

## Time Complexity
- O(d * (n + k)) (where d is the number of digits, n is the number of elements, and k is the base of the number system/range of digits)

## Space Complexity
- O(n + k)

## Implementations (LSD Radix Sort using Counting Sort)

### JavaScript
```javascript
function getDigit(num, place) {
    return Math.floor(Math.abs(num) / Math.pow(10, place)) % 10;
}

function digitCount(num) {
    if (num === 0) return 1;
    return Math.floor(Math.log10(Math.abs(num))) + 1;
}

function mostDigits(nums) {
    let maxDigits = 0;
    for (let i = 0; i < nums.length; i++) {
        maxDigits = Math.max(maxDigits, digitCount(nums[i]));
    }
    return maxDigits;
}

function radixSort(nums) {
    let maxDigitCount = mostDigits(nums);
    for (let k = 0; k < maxDigitCount; k++) {
        let digitBuckets = Array.from({ length: 10 }, () => []);
        for (let i = 0; i < nums.length; i++) {
            let digit = getDigit(nums[i], k);
            digitBuckets[digit].push(nums[i]);
        }
        nums = [].concat(...digitBuckets);
    }
    return nums;
}

let arrJS = [23, 345, 5467, 12, 2345, 9852];
console.log("Radix Sort (JS):", radixSort(arrJS)); // [12, 23, 345, 2345, 5467, 9852]
```

### TypeScript
```typescript
function getDigitTS(num: number, place: number): number {
    return Math.floor(Math.abs(num) / Math.pow(10, place)) % 10;
}

function digitCountTS(num: number): number {
    if (num === 0) return 1;
    return Math.floor(Math.log10(Math.abs(num))) + 1;
}

function mostDigitsTS(nums: number[]): number {
    let maxDigits = 0;
    for (let i = 0; i < nums.length; i++) {
        maxDigits = Math.max(maxDigits, digitCountTS(nums[i]));
    }
    return maxDigits;
}

function radixSortTS(nums: number[]): number[] {
    let maxDigitCount = mostDigitsTS(nums);
    for (let k = 0; k < maxDigitCount; k++) {
        let digitBuckets: number[][] = Array.from({ length: 10 }, () => []);
        for (let i = 0; i < nums.length; i++) {
            let digit = getDigitTS(nums[i], k);
            digitBuckets[digit].push(nums[i]);
        }
        nums = ([] as number[]).concat(...digitBuckets);
    }
    return nums;
}

let arrTS: number[] = [23, 345, 5467, 12, 2345, 9852];
console.log("Radix Sort (TS):", radixSortTS(arrTS)); // [12, 23, 345, 2345, 5467, 9852]
```

### Python
```python
def get_digit(num, place):
    return (abs(num) // (10 ** place)) % 10

def digit_count(num):
    if num == 0: return 1
    return len(str(abs(num)))

def most_digits(nums):
    max_digits = 0
    for num in nums:
        max_digits = max(max_digits, digit_count(num))
    return max_digits

def radix_sort(nums):
    max_digit_count = most_digits(nums)
    for k in range(max_digit_count):
        digit_buckets = [[] for _ in range(10)]
        for num in nums:
            digit = get_digit(num, k)
            digit_buckets[digit].append(num)
        nums = [num for bucket in digit_buckets for num in bucket]
    return nums

arr_py = [23, 345, 5467, 12, 2345, 9852]
print("Radix Sort (Python):", radix_sort(arr_py)) # [12, 23, 345, 2345, 5467, 9852]
```

### PHP
```php
<?php

function getDigitPHP(int $num, int $place): int {
    return (int)floor(abs($num) / (10 ** $place)) % 10;
}

function digitCountPHP(int $num): int {
    if ($num === 0) return 1;
    return (int)floor(log10(abs($num))) + 1;
}

function mostDigitsPHP(array $nums): int {
    $maxDigits = 0;
    foreach ($nums as $num) {
        $maxDigits = max($maxDigits, digitCountPHP($num));
    }
    return $maxDigits;
}

function radixSortPHP(array $nums): array {
    $maxDigitCount = mostDigitsPHP($nums);
    for ($k = 0; $k < $maxDigitCount; $k++) {
        $digitBuckets = array_fill(0, 10, []);
        foreach ($nums as $num) {
            $digit = getDigitPHP($num, $k);
            $digitBuckets[$digit][] = $num;
        }
        $nums = array_merge(...$digitBuckets);
    }
    return $nums;
}

$arrPHP = [23, 345, 5467, 12, 2345, 9852];
echo "Radix Sort (PHP): ";
print_r(radixSortPHP($arrPHP)); // Array ( [0] => 12 [1] => 23 [2] => 345 [3] => 2345 [4] => 5467 [5] => 9852 )

?>
```