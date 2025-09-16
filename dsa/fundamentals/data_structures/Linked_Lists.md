# Linked Lists

## Description
A linked list is a linear data structure where elements are not stored at contiguous memory locations. Instead, each element (node) is an object that contains a data field and a reference (or link) to the next node in the sequence.

## When to Use
- When you need dynamic data structures where elements can be easily inserted or deleted without reorganizing the entire structure.
- When the number of elements is unknown or changes frequently.
- When memory allocation needs to be flexible, as nodes can be scattered throughout memory.

## Implementations

### JavaScript (Singly Linked List)
```javascript
class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }

    // Add a node to the end of the list
    add(data) {
        let node = new Node(data);
        if (this.head === null) {
            this.head = node;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = node;
        }
        this.size++;
    }

    // Print the list
    printList() {
        let current = this.head;
        let str = "";
        while (current) {
            str += current.data + " ";
            current = current.next;
        }
        console.log(str);
    }
}

let ll = new LinkedList();
ll.add(10);
ll.add(20);
ll.add(30);
ll.printList(); // 10 20 30 
```

### TypeScript (Singly Linked List)
```typescript
class Node<T> {
    data: T;
    next: Node<T> | null;

    constructor(data: T) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList<T> {
    head: Node<T> | null;
    size: number;

    constructor() {
        this.head = null;
        this.size = 0;
    }

    add(data: T): void {
        let node = new Node(data);
        if (this.head === null) {
            this.head = node;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = node;
        }
        this.size++;
    }

    printList(): void {
        let current = this.head;
        let str = "";
        while (current) {
            str += current.data + " ";
            current = current.next;
        }
        console.log(str);
    }
}

let ll = new LinkedList<number>();
ll.add(10);
ll.add(20);
ll.add(30);
ll.printList(); // 10 20 30 
```

### Python (Singly Linked List)
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def add(self, data):
        new_node = Node(data)
        if self.head is None:
            self.head = new_node
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = new_node

    def print_list(self):
        current = self.head
        while current:
            print(current.data, end=" ")
            current = current.next
        print()

ll = LinkedList()
ll.add(10)
ll.add(20)
ll.add(30);
ll.print_list() # 10 20 30
```

### PHP (Singly Linked List)
```php
<?php

class Node {
    public $data;
    public $next;

    public function __construct($data) {
        $this->data = $data;
        $this->next = null;
    }
}

class LinkedList {
    public $head;

    public function __construct() {
        $this->head = null;
    }

    public function add($data) {
        $newNode = new Node($data);
        if ($this->head === null) {
            $this->head = $newNode;
        } else {
            $current = $this->head;
            while ($current->next !== null) {
                $current = $current->next;
            }
            $current->next = $newNode;
        }
    }

    public function printList() {
        $current = $this->head;
        $output = "";
        while ($current !== null) {
            $output .= $current->data . " ";
            $current = $current->next;
        }
        echo $output . "\n";
    }
}

$ll = new LinkedList();
$ll->add(10);
$ll->add(20);
$ll->add(30);
$ll->printList(); // 10 20 30 

?>
```