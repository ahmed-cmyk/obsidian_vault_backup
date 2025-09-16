# Fenwick Trees (Binary Indexed Trees - BIT)

## Description
A Fenwick Tree, also known as a Binary Indexed Tree (BIT), is a data structure that can efficiently update elements and calculate prefix sums in a table of numbers. It achieves this by representing numbers as a tree, where the value of each node is the sum of a range of numbers in the input array.

## When to Use
- When you need to perform both point updates and range sum queries (or other associative operations like min/max) on an array efficiently.
- In competitive programming for problems requiring dynamic prefix sums.
- When dealing with frequency arrays where elements are added or removed, and you need to query cumulative frequencies.

## Time Complexity
- **Build Tree:** O(n log n) (or O(n) if built iteratively)
- **Update (Point Update):** O(log n)
- **Query (Prefix Sum):** O(log n)

## Space Complexity
- O(n)

## Implementations (Prefix Sum and Point Update)

### JavaScript
```javascript
class FenwickTree {
    constructor(arr) {
        this.n = arr.length;
        this.bit = new Array(this.n + 1).fill(0);
        for (let i = 0; i < this.n; i++) {
            this.update(i, arr[i]);
        }
    }

    // Update the value at index `idx` by `delta`
    update(idx, delta) {
        idx++; // 1-based indexing for BIT
        while (idx <= this.n) {
            this.bit[idx] += delta;
            idx += idx & (-idx);
        }
    }

    // Get the prefix sum up to index `idx` (inclusive)
    query(idx) {
        idx++; // 1-based indexing for BIT
        let sum = 0;
        while (idx > 0) {
            sum += this.bit[idx];
            idx -= idx & (-idx);
        }
        return sum;
    }

    // Get sum of range [l, r]
    rangeQuery(l, r) {
        return this.query(r) - this.query(l - 1);
    }
}

let arrJS = [1, 2, 3, 4, 5, 6, 7, 8];
let ftJS = new FenwickTree(arrJS);
console.log("Prefix sum up to index 3 (JS):", ftJS.query(3)); // 1+2+3+4 = 10
console.log("Range sum [2, 5] (JS):", ftJS.rangeQuery(2, 5)); // 3+4+5+6 = 18
ftJS.update(2, 10); // Update arr[2] from 3 to 13 (delta = 10)
console.log("Prefix sum up to index 3 after update (JS):", ftJS.query(3)); // 1+2+13+4 = 20
```

### TypeScript
```typescript
class FenwickTreeTS {
    private n: number;
    private bit: number[];

    constructor(arr: number[]) {
        this.n = arr.length;
        this.bit = new Array(this.n + 1).fill(0);
        for (let i = 0; i < this.n; i++) {
            this.update(i, arr[i]);
        }
    }

    update(idx: number, delta: number): void {
        idx++; // 1-based indexing for BIT
        while (idx <= this.n) {
            this.bit[idx] += delta;
            idx += idx & (-idx);
        }
    }

    query(idx: number): number {
        idx++; // 1-based indexing for BIT
        let sum = 0;
        while (idx > 0) {
            sum += this.bit[idx];
            idx -= idx & (-idx);
        }
        return sum;
    }

    rangeQuery(l: number, r: number): number {
        return this.query(r) - this.query(l - 1);
    }
}

let arrTS: number[] = [1, 2, 3, 4, 5, 6, 7, 8];
let ftTS = new FenwickTreeTS(arrTS);
console.log("Prefix sum up to index 3 (TS):", ftTS.query(3)); // 1+2+3+4 = 10
console.log("Range sum [2, 5] (TS):", ftTS.rangeQuery(2, 5)); // 3+4+5+6 = 18
ftTS.update(2, 10); // Update arr[2] from 3 to 13 (delta = 10)
console.log("Prefix sum up to index 3 after update (TS):", ftTS.query(3)); // 1+2+13+4 = 20
```

### Python
```python
class FenwickTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.bit = [0] * (self.n + 1)
        for i in range(self.n):
            self.update(i, arr[i])

    def update(self, idx, delta):
        idx += 1 # 1-based indexing for BIT
        while idx <= self.n:
            self.bit[idx] += delta
            idx += idx & (-idx)

    def query(self, idx):
        idx += 1 # 1-based indexing for BIT
        s = 0
        while idx > 0:
            s += self.bit[idx]
            idx -= idx & (-idx)
        return s

    def range_query(self, l, r):
        return self.query(r) - self.query(l - 1)

arr_py = [1, 2, 3, 4, 5, 6, 7, 8]
ft_py = FenwickTree(arr_py)
print("Prefix sum up to index 3 (Python):", ft_py.query(3)) # 1+2+3+4 = 10
print("Range sum [2, 5] (Python):", ft_py.range_query(2, 5)) # 3+4+5+6 = 18
ft_py.update(2, 10) # Update arr[2] from 3 to 13 (delta = 10)
print("Prefix sum up to index 3 after update (Python):", ft_py.query(3)) # 1+2+13+4 = 20
```

### PHP
```php
<?php

class FenwickTreePHP {
    private $n;
    private $bit;

    public function __construct(array $arr) {
        $this->n = count($arr);
        $this->bit = array_fill(0, $this->n + 1, 0);
        for ($i = 0; $i < $this->n; $i++) {
            $this->update($i, $arr[$i]);
        }
    }

    public function update(int $idx, int $delta): void {
        $idx++; // 1-based indexing for BIT
        while ($idx <= $this->n) {
            $this->bit[$idx] += $delta;
            $idx += ($idx & (-$idx));
        }
    }

    public function query(int $idx): int {
        $idx++; // 1-based indexing for BIT
        $sum = 0;
        while ($idx > 0) {
            $sum += $this->bit[$idx];
            $idx -= ($idx & (-$idx));
        }
        return $sum;
    }

    public function rangeQuery(int $l, int $r): int {
        return $this->query($r) - $this->query($l - 1);
    }
}

$arrPHP = [1, 2, 3, 4, 5, 6, 7, 8];
$ftPHP = new FenwickTreePHP($arrPHP);
echo "Prefix sum up to index 3 (PHP): " . $ftPHP->query(3) . "\n"; // 1+2+3+4 = 10
echo "Range sum [2, 5] (PHP): " . $ftPHP->rangeQuery(2, 5) . "\n"; // 3+4+5+6 = 18
$ftPHP->update(2, 10); // Update arr[2] from 3 to 13 (delta = 10)
echo "Prefix sum up to index 3 after update (PHP): " . $ftPHP->query(3) . "\n"; // 1+2+13+4 = 20

?>
```
```