# Trees (Binary Trees)

## Description
A tree is a non-linear data structure that simulates a hierarchical tree structure, with a root value and subtrees of children with a parent node, represented as a set of linked nodes. A binary tree is a tree data structure in which each node has at most two children, referred to as the left child and the right child.

## When to Use
- When representing hierarchical data, such as file systems, organizational charts, or XML/HTML documents.
- For efficient searching and sorting (e.g., Binary Search Trees).
- In routing algorithms and network topologies.
- For parsing expressions and syntax trees in compilers.

## Time Complexity (Binary Search Tree - Average Case)
- **Insertion:** O(log n)
- **Deletion:** O(log n)
- **Search:** O(log n)

## Time Complexity (Binary Search Tree - Worst Case)
- **Insertion:** O(n)
- **Deletion:** O(n)
- **Search:** O(n)

## Space Complexity
- O(n) (where n is the number of nodes)

## Implementations (Binary Search Tree - Simplified)

### JavaScript
```javascript
class Node {
    constructor(data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }

    insert(data) {
        let newNode = new Node(data);
        if (this.root === null) {
            this.root = newNode;
        } else {
            this.insertNode(this.root, newNode);
        }
    }

    insertNode(node, newNode) {
        if (newNode.data < node.data) {
            if (node.left === null) {
                node.left = newNode;
            } else {
                this.insertNode(node.left, newNode);
            }
        } else {
            if (node.right === null) {
                node.right = newNode;
            } else {
                this.insertNode(node.right, newNode);
            }
        }
    }

    search(node, data) {
        if (node === null) {
            return null;
        } else if (data < node.data) {
            return this.search(node.left, data);
        } else if (data > node.data) {
            return this.search(node.right, data);
        } else {
            return node;
        }
    }

    // In-order traversal (for demonstration)
    inorder(node) {
        if (node !== null) {
            this.inorder(node.left);
            console.log(node.data);
            this.inorder(node.right);
        }
    }
}

let bst = new BinarySearchTree();
bst.insert(15);
bst.insert(25);
bst.insert(10);
bst.insert(7);
bst.insert(22);
bst.insert(17);
bst.insert(13);

console.log("In-order traversal (JS):");
bst.inorder(bst.root);
console.log("Search for 22 (JS):", bst.search(bst.root, 22) ? "Found" : "Not Found");
```

### TypeScript
```typescript
class NodeTS<T> {
    data: T;
    left: NodeTS<T> | null;
    right: NodeTS<T> | null;

    constructor(data: T) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTreeTS<T> {
    root: NodeTS<T> | null;

    constructor() {
        this.root = null;
    }

    insert(data: T): void {
        let newNode = new NodeTS(data);
        if (this.root === null) {
            this.root = newNode;
        } else {
            this.insertNode(this.root, newNode);
        }
    }

    private insertNode(node: NodeTS<T>, newNode: NodeTS<T>): void {
        if (newNode.data < node.data) {
            if (node.left === null) {
                node.left = newNode;
            } else {
                this.insertNode(node.left, newNode);
            }
        } else {
            if (node.right === null) {
                node.right = newNode;
            }
        }
    }

    search(node: NodeTS<T> | null, data: T): NodeTS<T> | null {
        if (node === null) {
            return null;
        } else if (data < node.data) {
            return this.search(node.left, data);
        } else if (data > node.data) {
            return this.search(node.right, data);
        } else {
            return node;
        }
    }

    inorder(node: NodeTS<T> | null): void {
        if (node !== null) {
            this.inorder(node.left);
            console.log(node.data);
            this.inorder(node.right);
        }
    }
}

let bstTS = new BinarySearchTreeTS<number>();
bstTS.insert(15);
bstTS.insert(25);
bstTS.insert(10);
bstTS.insert(7);
bstTS.insert(22);
bstTS.insert(17);
bstTS.insert(13);

console.log("In-order traversal (TS):");
bstTS.inorder(bstTS.root);
console.log("Search for 22 (TS):", bstTS.search(bstTS.root, 22) ? "Found" : "Not Found");
```

### Python
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, data):
        new_node = Node(data)
        if self.root is None:
            self.root = new_node
        else:
            self._insert_node(self.root, new_node)

    def _insert_node(self, node, new_node):
        if new_node.data < node.data:
            if node.left is None:
                node.left = new_node
            else:
                self._insert_node(node.left, new_node)
        else:
            if node.right is None:
                node.right = new_node
            else:
                self._insert_node(node.right, new_node)

    def search(self, node, data):
        if node is None:
            return None
        elif data < node.data:
            return self.search(node.left, data)
        elif data > node.data:
            return self.search(node.right, data)
        else:
            return node

    def inorder(self, node):
        if node:
            self.inorder(node.left)
            print(node.data)
            self.inorder(node.right)

bst = BinarySearchTree()
bst.insert(15)
bst.insert(25)
bst.insert(10)
bst.insert(7)
bst.insert(22)
bst.insert(17)
bst.insert(13)

print("In-order traversal (Python):")
bst.inorder(bst.root)
print("Search for 22 (Python):", "Found" if bst.search(bst.root, 22) else "Not Found")
```

### PHP
```php
<?php

class NodePHP {
    public $data;
    public $left;
    public $right;

    public function __construct($data) {
        $this->data = $data;
        $this->left = null;
        $this->right = null;
    }
}

class BinarySearchTreePHP {
    public $root;

    public function __construct() {
        $this->root = null;
    }

    public function insert($data) {
        $newNode = new NodePHP($data);
        if ($this->root === null) {
            $this->root = $newNode;
        } else {
            $this->insertNode($this->root, $newNode);
        }
    }

    private function insertNode($node, $newNode) {
        if ($newNode->data < $node->data) {
            if ($node->left === null) {
                $node->left = $newNode;
            } else {
                $this->insertNode($node->left, $newNode);
            }
        } else {
            if ($node->right === null) {
                $node->right = $newNode;
            } else {
                $this->insertNode($node->right, $newNode);
            }
        }
    }

    public function search($node, $data) {
        if ($node === null) {
            return null;
        } else if ($data < $node->data) {
            return $this->search($node->left, $data);
        } else if ($data > $node->data) {
            return $this->search($node->right, $data);
        }
    } else {
            return $node;
        }
    }

    public function inorder($node) {
        if ($node !== null) {
            $this->inorder($node->left);
            echo $node->data . "\n";
            $this->inorder($node->right);
        }
    }
}

$bstPHP = new BinarySearchTreePHP();
$bstPHP->insert(15);
$bstPHP->insert(25);
$bstPHP->insert(10);
$bstPHP->insert(7);
$bstPHP->insert(22);
$bstPHP->insert(17);
$bstPHP->insert(13);

echo "In-order traversal (PHP):\n";
$bstPHP->inorder($bstPHP->root);
echo "Search for 22 (PHP): " . ($bstPHP->search($bstPHP->root, 22) ? "Found" : "Not Found") . "\n";

?>
```
```