# Heaps (Binary Heap)

## Description
A heap is a specialized tree-based data structure that satisfies the heap property. In a max-heap, for any given node C, if P is a parent node of C, then the value of P is greater than or equal to the value of C. In a min-heap, the value of P is less than or equal to the value of C. Heaps are commonly implemented as arrays.

## When to Use
- For implementing priority queues, where elements are retrieved based on their priority.
- In heap sort algorithm.
- For finding the k-th smallest/largest element in an array.
- In graph algorithms like Dijkstra's and Prim's for efficient edge selection.

## Time Complexity (Binary Heap)
- **Insertion (push):** O(log n)
- **Deletion (pop max/min):** O(log n)
- **Peek (get max/min):** O(1)
- **Build Heap:** O(n)

## Space Complexity
- O(n) (where n is the number of elements in the heap)

## Implementations (Min-Heap)

### JavaScript
```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }

    getParentIndex(i) {
        return Math.floor((i - 1) / 2);
    }

    getLeftChildIndex(i) {
        return 2 * i + 1;
    }

    getRightChildIndex(i) {
        return 2 * i + 2;
    }

    hasParent(i) {
        return this.getParentIndex(i) >= 0;
    }

    hasLeftChild(i) {
        return this.getLeftChildIndex(i) < this.heap.length;
    }

    hasRightChild(i) {
        return this.getRightChildIndex(i) < this.heap.length;
    }

    getParent(i) {
        return this.heap[this.getParentIndex(i)];
    }

    getLeftChild(i) {
        return this.heap[this.getLeftChildIndex(i)];
    }

    getRightChild(i) {
        return this.heap[this.getRightChildIndex(i)];
    }

    swap(i, j) {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }

    peek() {
        if (this.heap.length === 0) return null;
        return this.heap[0];
    }

    insert(item) {
        this.heap.push(item);
        this.heapifyUp();
    }

    extractMin() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();

        const item = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.heapifyDown();
        return item;
    }

    heapifyUp() {
        let index = this.heap.length - 1;
        while (this.hasParent(index) && this.getParent(index) > this.heap[index]) {
            this.swap(this.getParentIndex(index), index);
            index = this.getParentIndex(index);
        }
    }

    heapifyDown() {
        let index = 0;
        while (this.hasLeftChild(index)) {
            let smallerChildIndex = this.getLeftChildIndex(index);
            if (this.hasRightChild(index) && this.getRightChild(index) < this.getLeftChild(index)) {
                smallerChildIndex = this.getRightChildIndex(index);
            }

            if (this.heap[index] < this.heap[smallerChildIndex]) {
                break;
            } else {
                this.swap(index, smallerChildIndex);
            }
            index = smallerChildIndex;
        }
    }

    printHeap() {
        console.log(this.heap);
    }
}

let minHeapJS = new MinHeap();
minHeapJS.insert(10);
minHeapJS.insert(4);
minHeapJS.insert(15);
minHeapJS.insert(20);
minHeapJS.insert(1);
minHeapJS.printHeap(); // [1, 4, 15, 20, 10]
console.log("Min element (JS):", minHeapJS.extractMin()); // 1
minHeapJS.printHeap(); // [4, 10, 15, 20]
```

### TypeScript
```typescript
class MinHeapTS<T> {
    private heap: T[] = [];

    private getParentIndex(i: number): number {
        return Math.floor((i - 1) / 2);
    }

    private getLeftChildIndex(i: number): number {
        return 2 * i + 1;
    }

    private getRightChildIndex(i: number): number {
        return 2 * i + 2;
    }

    private hasParent(i: number): boolean {
        return this.getParentIndex(i) >= 0;
    }

    private hasLeftChild(i: number): boolean {
        return this.getLeftChildIndex(i) < this.heap.length;
    }

    private hasRightChild(i: number): boolean {
        return this.getRightChildIndex(i) < this.heap.length;
    }

    private getParent(i: number): T {
        return this.heap[this.getParentIndex(i)];
    }

    private getLeftChild(i: number): T {
        return this.heap[this.getLeftChildIndex(i)];
    }

    private getRightChild(i: number): T {
        return this.heap[this.getRightChildIndex(i)];
    }

    private swap(i: number, j: number): void {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }

    peek(): T | null {
        if (this.heap.length === 0) return null;
        return this.heap[0];
    }

    insert(item: T): void {
        this.heap.push(item);
        this.heapifyUp();
    }

    extractMin(): T | null {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop()!;

        const item = this.heap[0];
        this.heap[0] = this.heap.pop()!;
        this.heapifyDown();
        return item;
    }

    private heapifyUp(): void {
        let index = this.heap.length - 1;
        while (this.hasParent(index) && this.getParent(index) > this.heap[index]) {
            this.swap(this.getParentIndex(index), index);
            index = this.getParentIndex(index);
        }
    }

    private heapifyDown(): void {
        let index = 0;
        while (this.hasLeftChild(index)) {
            let smallerChildIndex = this.getLeftChildIndex(index);
            if (this.hasRightChild(index) && this.getRightChild(index) < this.getLeftChild(index)) {
                smallerChildIndex = this.getRightChildIndex(index);
            }

            if (this.heap[index] < this.heap[smallerChildIndex]) {
                break;
            } else {
                this.swap(index, smallerChildIndex);
            }
            index = smallerChildIndex;
        }
    }

    printHeap(): void {
        console.log(this.heap);
    }
}

let minHeapTS = new MinHeapTS<number>();
minHeapTS.insert(10);
minHeapTS.insert(4);
minHeapTS.insert(15);
minHeapTS.insert(20);
minHeapTS.insert(1);
minHeapTS.printHeap(); // [1, 4, 15, 20, 10]
console.log("Min element (TS):", minHeapTS.extractMin()); // 1
minHeapTS.printHeap(); // [4, 10, 15, 20]
```

### Python
```python
import heapq

class MinHeapPy:
    def __init__(self):
        self.heap = []

    def insert(self, item):
        heapq.heappush(self.heap, item)

    def extract_min(self):
        if not self.heap:
            return None
        return heapq.heappop(self.heap)

    def peek(self):
        if not self.heap:
            return None
        return self.heap[0]

    def print_heap(self):
        print(self.heap)

minHeapPy = MinHeapPy()
minHeapPy.insert(10)
minHeapPy.insert(4)
minHeapPy.insert(15)
minHeapPy.insert(20)
minHeapPy.insert(1);
minHeapPy.print_heap() # [1, 4, 15, 20, 10]
print("Min element (Python):", minHeapPy.extract_min()) # 1
minHeapPy.print_heap() # [4, 10, 15, 20]
```

### PHP
```php
<?php

class MinHeapPHP extends SplMinHeap {
    public function compare($value1, $value2): int {
        return $value1 <=> $value2;
    }

    public function insert($value): void {
        parent::insert($value);
    }

    public function extractMin() {
        if ($this->isEmpty()) {
            return null;
        }
        return parent::extract();
    }

    public function peek() {
        if ($this->isEmpty()) {
            return null;
        }
        return parent::top();
    }

    public function printHeap() {
        $tempHeap = clone $this;
        $elements = [];
        while (!$tempHeap->isEmpty()) {
            $elements[] = $tempHeap->extract();
        }
        echo "[" . implode(", ", $elements) . "]\n";
    }
}

$minHeapPHP = new MinHeapPHP();
$minHeapPHP->insert(10);
$minHeapPHP->insert(4);
$minHeapPHP->insert(15);
$minHeapPHP->insert(20);
$minHeapPHP->insert(1);
$minHeapPHP->printHeap(); // [1, 4, 10, 15, 20]
echo "Min element (PHP): " . $minHeapPHP->extractMin() . "\n"; // 1
$minHeapPHP->printHeap(); // [4, 10, 15, 20]

?>
```