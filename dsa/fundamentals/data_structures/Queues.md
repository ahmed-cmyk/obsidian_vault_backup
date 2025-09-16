# Queues

## Description
A queue is a linear data structure that follows a particular order in which the operations are performed. The order is FIFO (First In First Out). The two main operations are `enqueue` (add an element to the rear) and `dequeue` (remove an element from the front).

## When to Use
- When you need to process data in the order it was received, such as task scheduling, breadth-first search (BFS), or managing requests in a server.
- For managing shared resources in a fair manner.
- In print spooling, where documents are printed in the order they were sent.

## Implementations

### JavaScript
```javascript
class Queue {
    constructor() {
        this.items = [];
    }

    // Add element to the queue
    enqueue(element) {
        this.items.push(element);
    }

    // Remove element from the queue
    dequeue() {
        if (this.isEmpty())
            return "Underflow";
        return this.items.shift();
    }

    // View the front element
    front() {
        if (this.isEmpty())
            return "No elements in Queue";
        return this.items[0];
    }

    // Check if the queue is empty
    isEmpty() {
        return this.items.length == 0;
    }

    // Get the size of the queue
    size() {
        return this.items.length;
    }

    // Print the queue
    printQueue() {
        let str = "";
        for (let i = 0; i < this.items.length; i++)
            str += this.items[i] + " ";
        return str;
    }
}

let queue = new Queue();
queue.enqueue(10);
queue.enqueue(20);
console.log(queue.printQueue()); // 10 20 
console.log(queue.dequeue()); // 10
console.log(queue.front()); // 20
```

### TypeScript
```typescript
class Queue<T> {
    private items: T[];

    constructor() {
        this.items = [];
    }

    enqueue(element: T): void {
        this.items.push(element);
    }

    dequeue(): T | string {
        if (this.isEmpty())
            return "Underflow";
        return this.items.shift()!;
    }

    front(): T | string {
        if (this.isEmpty())
            return "No elements in Queue";
        return this.items[0];
    }

    isEmpty(): boolean {
        return this.items.length == 0;
    }

    size(): number {
        return this.items.length;
    }

    printQueue(): string {
        let str = "";
        for (let i = 0; i < this.items.length; i++)
            str += this.items[i] + " ";
        return str;
    }
}

let queue = new Queue<number>();
queue.enqueue(10);
queue.enqueue(20);
console.log(queue.printQueue()); // 10 20 
console.log(queue.dequeue()); // 10
console.log(queue.front()); // 20
```

### Python
```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()

    def enqueue(self, element):
        self.items.append(element);

    def dequeue(self):
        if self.is_empty():
            return "Underflow"
        return self.items.popleft()

    def front(self):
        if self.is_empty():
            return "No elements in Queue"
        return self.items[0]

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)

    def print_queue(self):
        print(list(self.items))

queue = Queue()
queue.enqueue(10)
queue.enqueue(20)
queue.print_queue() # deque([10, 20])
print(queue.dequeue()) # 10
print(queue.front()) # 20
```

### PHP
```php
<?php

class Queue {
    private $items = [];

    public function enqueue($element) {
        array_push($this->items, $element);
    }

    public function dequeue() {
        if ($this->isEmpty())
            return "Underflow";
        return array_shift($this->items);
    }

    public function front() {
        if ($this->isEmpty())
            return "No elements in Queue";
        return $this->items[0];
    }

    public function isEmpty() {
        return empty($this->items);
    }

    public function size() {
        return count($this->items);
    }

    public function printQueue() {
        echo implode(" ", $this->items) . "\n";
    }
}

$queue = new Queue();
$queue->enqueue(10);
$queue->enqueue(20);
$queue->printQueue(); // 10 20 
echo $queue->dequeue() . "\n"; // 10
echo $queue->front() . "\n"; // 20

?>
```