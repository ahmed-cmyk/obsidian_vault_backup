# Segment Trees

## Description
A Segment Tree is a tree data structure used for storing information about intervals or segments. It allows querying on segments (e.g., sum, min, max) in an array efficiently. Each node in the segment tree represents an interval. The root represents the entire array, and each leaf node represents a single element of the array. Internal nodes represent the union of their children's intervals.

## When to Use
- When you need to perform range queries (e.g., sum, minimum, maximum) and range updates on an array efficiently.
- In competitive programming for problems involving intervals.
- For problems like range minimum query (RMQ), range sum query (RSQ), etc.

## Time Complexity
- **Build Tree:** O(n)
- **Query (Range Sum/Min/Max):** O(log n)
- **Update (Point Update):** O(log n)

## Space Complexity
- O(n) (typically 2n or 4n depending on implementation)

## Implementations (Range Sum Query)

### JavaScript
```javascript
class SegmentTree {
    constructor(arr) {
        this.n = arr.length;
        this.tree = new Array(4 * this.n).fill(0);
        this.build(arr, 0, 0, this.n - 1);
    }

    build(arr, treeIndex, low, high) {
        if (low === high) {
            this.tree[treeIndex] = arr[low];
            return;
        }

        let mid = Math.floor((low + high) / 2);
        this.build(arr, 2 * treeIndex + 1, low, mid);
        this.build(arr, 2 * treeIndex + 2, mid + 1, high);
        this.tree[treeIndex] = this.tree[2 * treeIndex + 1] + this.tree[2 * treeIndex + 2];
    }

    query(treeIndex, low, high, queryLow, queryHigh) {
        // No overlap
        if (queryLow > high || queryHigh < low) {
            return 0;
        }

        // Complete overlap
        if (queryLow <= low && high <= queryHigh) {
            return this.tree[treeIndex];
        }

        // Partial overlap
        let mid = Math.floor((low + high) / 2);
        let leftSum = this.query(2 * treeIndex + 1, low, mid, queryLow, queryHigh);
        let rightSum = this.query(2 * treeIndex + 2, mid + 1, high, queryLow, queryHigh);
        return leftSum + rightSum;
    }

    update(treeIndex, low, high, index, value) {
        if (low === high) {
            this.tree[treeIndex] = value;
            return;
        }

        let mid = Math.floor((low + high) / 2);
        if (index <= mid) {
            this.update(2 * treeIndex + 1, low, mid, index, value);
        } else {
            this.update(2 * treeIndex + 2, mid + 1, high, index, value);
        }
        this.tree[treeIndex] = this.tree[2 * treeIndex + 1] + this.tree[2 * treeIndex + 2];
    }
}

let arrJS = [1, 3, 5, 7, 9, 11];
let segTreeJS = new SegmentTree(arrJS);
console.log("Range sum [1, 4] (JS):", segTreeJS.query(0, 0, arrJS.length - 1, 1, 4)); // 3 + 5 + 7 + 9 = 24
segTreeJS.update(0, 0, arrJS.length - 1, 2, 10); // Update arr[2] from 5 to 10
console.log("Range sum [1, 4] after update (JS):", segTreeJS.query(0, 0, arrJS.length - 1, 1, 4)); // 3 + 10 + 7 + 9 = 29
```

### TypeScript
```typescript
class SegmentTreeTS {
    private n: number;
    private tree: number[];

    constructor(arr: number[]) {
        this.n = arr.length;
        this.tree = new Array(4 * this.n).fill(0);
        this.build(arr, 0, 0, this.n - 1);
    }

    private build(arr: number[], treeIndex: number, low: number, high: number): void {
        if (low === high) {
            this.tree[treeIndex] = arr[low];
            return;
        }

        let mid = Math.floor((low + high) / 2);
        this.build(arr, 2 * treeIndex + 1, low, mid);
        this.build(arr, 2 * treeIndex + 2, mid + 1, high);
        this.tree[treeIndex] = this.tree[2 * treeIndex + 1] + this.tree[2 * treeIndex + 2];
    }

    query(treeIndex: number, low: number, high: number, queryLow: number, queryHigh: number): number {
        // No overlap
        if (queryLow > high || queryHigh < low) {
            return 0;
        }

        // Complete overlap
        if (queryLow <= low && high <= queryHigh) {
            return this.tree[treeIndex];
        }

        // Partial overlap
        let mid = Math.floor((low + high) / 2);
        let leftSum = this.query(2 * treeIndex + 1, low, mid, queryLow, queryHigh);
        let rightSum = this.query(2 * treeIndex + 2, mid + 1, high, queryLow, queryHigh);
        return leftSum + rightSum;
    }

    update(treeIndex: number, low: number, high: number, index: number, value: number): void {
        if (low === high) {
            this.tree[treeIndex] = value;
            return;
        }

        let mid = Math.floor((low + high) / 2);
        if (index <= mid) {
            this.update(2 * treeIndex + 1, low, mid, index, value);
        } else {
            this.update(2 * treeIndex + 2, mid + 1, high, index, value);
        }
        this.tree[treeIndex] = this.tree[2 * treeIndex + 1] + this.tree[2 * treeIndex + 2];
    }
}

let arrTS: number[] = [1, 3, 5, 7, 9, 11];
let segTreeTS = new SegmentTreeTS(arrTS);
console.log("Range sum [1, 4] (TS):", segTreeTS.query(0, 0, arrTS.length - 1, 1, 4)); // 3 + 5 + 7 + 9 = 24
segTreeTS.update(0, 0, arrTS.length - 1, 2, 10); // Update arr[2] from 5 to 10
console.log("Range sum [1, 4] after update (TS):", segTreeTS.query(0, 0, arrTS.length - 1, 1, 4)); // 3 + 10 + 7 + 9 = 29
```

### Python
```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self._build(arr, 0, 0, self.n - 1)

    def _build(self, arr, tree_index, low, high):
        if low == high:
            self.tree[tree_index] = arr[low]
            return

        mid = (low + high) // 2
        self._build(arr, 2 * tree_index + 1, low, mid)
        self._build(arr, 2 * tree_index + 2, mid + 1, high)
        self.tree[tree_index] = self.tree[2 * tree_index + 1] + self.tree[2 * tree_index + 2]

    def query(self, tree_index, low, high, query_low, query_high):
        # No overlap
        if query_low > high or query_high < low:
            return 0

        # Complete overlap
        if query_low <= low and high <= query_high:
            return self.tree[tree_index]

        # Partial overlap
        mid = (low + high) // 2
        left_sum = self.query(2 * tree_index + 1, low, mid, query_low, query_high)
        right_sum = self.query(2 * tree_index + 2, mid + 1, high, query_low, query_high)
        return left_sum + right_sum

    def update(self, tree_index, low, high, index, value):
        if low == high:
            self.tree[tree_index] = value
            return

        mid = (low + high) // 2
        if index <= mid:
            self.update(2 * tree_index + 1, low, mid, index, value)
        else:
            self.update(2 * tree_index + 2, mid + 1, high, index, value)
        self.tree[tree_index] = self.tree[2 * tree_index + 1] + self.tree[2 * tree_index + 2]

arr_py = [1, 3, 5, 7, 9, 11]
seg_tree_py = SegmentTree(arr_py);
print("Range sum [1, 4] (Python):", seg_tree_py.query(0, 0, len(arr_py) - 1, 1, 4)) # 3 + 5 + 7 + 9 = 24
seg_tree_py.update(0, 0, len(arr_py) - 1, 2, 10) # Update arr[2] from 5 to 10
print("Range sum [1, 4] after update (Python):", seg_tree_py.query(0, 0, len(arr_py) - 1, 1, 4)) # 3 + 10 + 7 + 9 = 29
```

### PHP
```php
<?php

class SegmentTreePHP {
    private $n;
    private $tree;

    public function __construct(array $arr) {
        $this->n = count($arr);
        $this->tree = array_fill(0, 4 * $this->n, 0);
        $this->build($arr, 0, 0, $this->n - 1);
    }

    private function build(array $arr, int $treeIndex, int $low, int $high): void {
        if ($low === $high) {
            $this->tree[$treeIndex] = $arr[$low];
            return;
        }

        $mid = floor(($low + $high) / 2);
        $this->build($arr, 2 * $treeIndex + 1, $low, $mid);
        $this->build($arr, 2 * $treeIndex + 2, $mid + 1, $high);
        $this->tree[$treeIndex] = $this->tree[2 * $treeIndex + 1] + $this->tree[2 * $treeIndex + 2];
    }

    public function query(int $treeIndex, int $low, int $high, int $queryLow, int $queryHigh): int {
        // No overlap
        if ($queryLow > $high || $queryHigh < $low) {
            return 0;
        }

        // Complete overlap
        if ($queryLow <= $low && $high <= $queryHigh) {
            return $this->tree[$treeIndex];
        }

        // Partial overlap
        $mid = floor(($low + $high) / 2);
        $leftSum = $this->query(2 * $treeIndex + 1, $low, $mid, $queryLow, $queryHigh);
        $rightSum = $this->query(2 * $treeIndex + 2, $mid + 1, $high, $queryLow, $queryHigh);
        return $leftSum + $rightSum;
    }

    public function update(int $treeIndex, int $low, int $high, int $index, int $value): void {
        if ($low === $high) {
            $this->tree[$treeIndex] = $value;
            return;
        }

        $mid = floor(($low + $high) / 2);
        if ($index <= $mid) {
            $this->update(2 * $treeIndex + 1, $low, $mid, $index, $value);
        } else {
            $this->update(2 * $treeIndex + 2, $mid + 1, $high, $index, $value);
        }
        $this->tree[$treeIndex] = $this->tree[2 * $treeIndex + 1] + $this->tree[2 * $treeIndex + 2];
    }
}

$arrPHP = [1, 3, 5, 7, 9, 11];
$segTreePHP = new SegmentTreePHP($arrPHP);
echo "Range sum [1, 4] (PHP): " . $segTreePHP->query(0, 0, count($arrPHP) - 1, 1, 4) . "\n"; // 3 + 5 + 7 + 9 = 24
$segTreePHP->update(0, 0, count($arrPHP) - 1, 2, 10); // Update arr[2] from 5 to 10
echo "Range sum [1, 4] after update (PHP): " . $segTreePHP->query(0, 0, count($arrPHP) - 1, 1, 4) . "\n"; // 3 + 10 + 7 + 9 = 29

?>
```
