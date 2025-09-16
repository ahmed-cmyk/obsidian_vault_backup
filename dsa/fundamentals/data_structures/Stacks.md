# Stacks

## Description
A stack is a linear data structure that follows a particular order in which the operations are performed. The order is LIFO (Last In First Out) or FILO (First In Last Out). The two main operations are `push` (add an element to the top) and `pop` (remove an element from the top).

## When to Use
- When you need to manage data in a LIFO manner, such as function call stacks, undo/redo functionality, or browser history.
- When you need to reverse a sequence of elements.
- For parsing expressions (e.g., converting infix to postfix).

## Implementations

### JavaScript
```javascript
class Stack {
    constructor() {
        this.items = [];
    }

    // Add element to the stack
    push(element) {
        this.items.push(element);
    }

    // Remove element from the stack
    pop() {
        if (this.items.length == 0)
            return "Underflow";
        return this.items.pop();
    }

    // View the top element
    peek() {
        return this.items[this.items.length - 1];
    }

    // Check if the stack is empty
    isEmpty() {
        return this.items.length == 0;
    }

    // Get the size of the stack
    size() {
        return this.items.length;
    }

    // Print the stack
    printStack() {
        let str = "";
        for (let i = 0; i < this.items.length; i++)
            str += this.items[i] + " ";
        return str;
    }
}

let stack = new Stack();
stack.push(10);
stack.push(20);
console.log(stack.printStack()); // 10 20 
console.log(stack.pop()); // 20
console.log(stack.peek()); // 10
```

### TypeScript
```typescript
class Stack<T> {
    private items: T[];

    constructor() {
        this.items = [];
    }

    push(element: T): void {
        this.items.push(element);
    }

    pop(): T | string {
        if (this.items.length == 0)
            return "Underflow";
        return this.items.pop()!;
    }

    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }

    isEmpty(): boolean {
        return this.items.length == 0;
    }

    size(): number {
        return this.items.length;
    }

    printStack(): string {
        let str = "";
        for (let i = 0; i < this.items.length; i++)
            str += this.items[i] + " ";
        return str;
    }
}

let stack = new Stack<number>();
stack.push(10);
stack.push(20);
console.log(stack.printStack()); // 10 20 
console.log(stack.pop()); // 20
console.log(stack.peek()); // 10
```

### Python
```python
class Stack:
    def __init__(self):
        self.items = []

    def push(self, element):
        self.items.append(element);

    def pop(self):
        if self.is_empty():
            return "Underflow"
        return self.items.pop()

    def peek(self):
        if self.is_empty():
            return "Stack is empty"
        return self.items[-1]

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)

    def print_stack(self):
        print(self.items)

stack = Stack()
stack.push(10)
stack.push(20)
stack.print_stack() # [10, 20]
print(stack.pop()) # 20
print(stack.peek()) # 10
```

### PHP
```php
<?php

class Stack {
    private $items = [];

    public function push($element) {
        array_push($this->items, $element);
    }

    public function pop() {
        if ($this->isEmpty())
            return "Underflow";
        return array_pop($this->items);
    }

    public function peek() {
        if ($this->isEmpty())
            return "Stack is empty";
        return $this->items[count($this->items) - 1];
    }

    public function isEmpty() {
        return empty($this->items);
    }

    public function size() {
        return count($this->items);
    }

    public function printStack() {
        echo implode(" ", $this->items) . "\n";
    }
}

$stack = new Stack();
$stack->push(10);
$stack->push(20);
$stack->printStack(); // 10 20 
echo $stack->pop() . "\n"; // 20
echo $stack->peek() . "\n"; // 10

?>
```