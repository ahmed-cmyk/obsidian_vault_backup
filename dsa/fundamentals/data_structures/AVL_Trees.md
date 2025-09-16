# AVL Trees

## Description
An AVL tree is a self-balancing binary search tree. It was the first such data structure to be invented. In an AVL tree, the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done to restore this property. This balancing ensures that the tree remains roughly balanced, leading to efficient search, insertion, and deletion operations.

## When to Use
- When you need a self-balancing binary search tree that guarantees O(log n) time complexity for search, insertion, and deletion operations.
- In databases and file systems where fast lookups and updates are critical.
- When the data is frequently updated and needs to maintain a balanced structure.
- As an underlying data structure for other data structures like sets and maps.

## Time Complexity
- **Search:** O(log n)
- **Insertion:** O(log n)
- **Deletion:** O(log n)

## Space Complexity
- O(n) (where n is the number of nodes)

## Implementations (Simplified Insertion)

### JavaScript
```javascript
class AVLNode {
    constructor(key) {
        this.key = key;
        this.left = null;
        this.right = null;
        this.height = 1;
    }
}

class AVLTree {
    constructor() {
        this.root = null;
    }

    getHeight(node) {
        if (node === null) {
            return 0;
        }
        return node.height;
    }

    getBalance(node) {
        if (node === null) {
            return 0;
        }
        return this.getHeight(node.left) - this.getHeight(node.right);
    }

    updateHeight(node) {
        node.height = 1 + Math.max(this.getHeight(node.left), this.getHeight(node.right));
    }

    rightRotate(y) {
        let x = y.left;
        let T2 = x.right;

        x.right = y;
        y.left = T2;

        this.updateHeight(y);
        this.updateHeight(x);

        return x;
    }

    leftRotate(x) {
        let y = x.right;
        let T2 = y.left;

        y.left = x;
        x.right = T2;

        this.updateHeight(x);
        this.updateHeight(y);

        return y;
    }

    insert(node, key) {
        if (node === null) {
            return new AVLNode(key);
        }

        if (key < node.key) {
            node.left = this.insert(node.left, key);
        } else if (key > node.key) {
            node.right = this.insert(node.right, key);
        } else {
            return node; // Duplicate keys not allowed
        }

        this.updateHeight(node);

        let balance = this.getBalance(node);

        // Left Left Case
        if (balance > 1 && key < node.left.key) {
            return this.rightRotate(node);
        }

        // Right Right Case
        if (balance < -1 && key > node.right.key) {
            return this.leftRotate(node);
        }

        // Left Right Case
        if (balance > 1 && key > node.left.key) {
            node.left = this.leftRotate(node.left);
            return this.rightRotate(node);
        }

        // Right Left Case
        if (balance < -1 && key < node.right.key) {
            node.right = this.rightRotate(node.right);
            return this.leftRotate(node);
        }

        return node;
    }

    insertKey(key) {
        this.root = this.insert(this.root, key);
    }

    // In-order traversal (for demonstration)
    inorder(node) {
        if (node !== null) {
            this.inorder(node.left);
            console.log(node.key);
            this.inorder(node.right);
        }
    }
}

let avl = new AVLTree();
avl.insertKey(10);
avl.insertKey(20);
avl.insertKey(30);
avl.insertKey(40);
avl.insertKey(50);
avl.insertKey(25);

console.log("In-order traversal of AVL Tree (JS):");
avl.inorder(avl.root);
// Expected: 10, 20, 25, 30, 40, 50 (sorted)
```

### TypeScript
```typescript
class AVLNodeTS<T> {
    key: T;
    left: AVLNodeTS<T> | null;
    right: AVLNodeTS<T> | null;
    height: number;

    constructor(key: T) {
        this.key = key;
        this.left = null;
        this.right = null;
        this.height = 1;
    }
}

class AVLTreeTS<T> {
    root: AVLNodeTS<T> | null;

    constructor() {
        this.root = null;
    }

    private getHeight(node: AVLNodeTS<T> | null): number {
        if (node === null) {
            return 0;
        }
        return node.height;
    }

    private getBalance(node: AVLNodeTS<T> | null): number {
        if (node === null) {
            return 0;
        }
        return this.getHeight(node.left) - this.getHeight(node.right);
    }

    private updateHeight(node: AVLNodeTS<T>): void {
        node.height = 1 + Math.max(this.getHeight(node.left), this.getHeight(node.right));
    }

    private rightRotate(y: AVLNodeTS<T>): AVLNodeTS<T> {
        let x = y.left!;
        let T2 = x.right;

        x.right = y;
        y.left = T2;

        this.updateHeight(y);
        this.updateHeight(x);

        return x;
    }

    private leftRotate(x: AVLNodeTS<T>): AVLNodeTS<T> {
        let y = x.right!;
        let T2 = y.left;

        y.left = x;
        x.right = T2;

        this.updateHeight(x);
        this.updateHeight(y);

        return y;
    }

    private insert(node: AVLNodeTS<T> | null, key: T): AVLNodeTS<T> {
        if (node === null) {
            return new AVLNodeTS(key);
        }

        if (key < node.key) {
            node.left = this.insert(node.left, key);
        } else if (key > node.key) {
            node.right = this.insert(node.right, key);
        } else {
            return node; // Duplicate keys not allowed
        }

        this.updateHeight(node);

        let balance = this.getBalance(node);

        // Left Left Case
        if (balance > 1 && key < node.left!.key) {
            return this.rightRotate(node);
        }

        // Right Right Case
        if (balance < -1 && key > node.right!.key) {
            return this.leftRotate(node);
        }

        // Left Right Case
        if (balance > 1 && key > node.left!.key) {
            node.left = this.leftRotate(node.left!);
            return this.rightRotate(node);
        }

        // Right Left Case
        if (balance < -1 && key < node.right!.key) {
            node.right = this.rightRotate(node.right!);
            return this.leftRotate(node);
        }

        return node;
    }

    insertKey(key: T): void {
        this.root = this.insert(this.root, key);
    }

    inorder(node: AVLNodeTS<T> | null): void {
        if (node !== null) {
            this.inorder(node.left);
            console.log(node.key);
            this.inorder(node.right);
        }
    }
}

let avlTS = new AVLTreeTS<number>();
avlTS.insertKey(10);
avlTS.insertKey(20);
avlTS.insertKey(30);
avlTS.insertKey(40);
avlTS.insertKey(50);
avlTS.insertKey(25);

console.log("In-order traversal of AVL Tree (TS):");
avlTS.inorder(avlTS.root);
// Expected: 10, 20, 25, 30, 40, 50 (sorted)
```

### Python
```python
class AVLNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

class AVLTree:
    def getHeight(self, node):
        if not node:
            return 0
        return node.height

    def getBalance(self, node):
        if not node:
            return 0
        return self.getHeight(node.left) - self.getHeight(node.right)

    def updateHeight(self, node):
        node.height = 1 + max(self.getHeight(node.left), self.getHeight(node.right))

    def rightRotate(self, y):
        x = y.left
        T2 = x.right

        x.right = y
        y.left = T2

        self.updateHeight(y)
        self.updateHeight(x)

        return x

    def leftRotate(self, x):
        y = x.right
        T2 = y.left

        y.left = x
        x.right = T2

        self.updateHeight(x)
        self.updateHeight(y)

        return y

    def insert(self, node, key):
        if not node:
            return AVLNode(key)

        if key < node.key:
            node.left = self.insert(node.left, key)
        elif key > node.key:
            node.right = self.insert(node.right, key)
        else:
            return node # Duplicate keys not allowed

        self.updateHeight(node);

        balance = self.getBalance(node)

        # Left Left Case
        if balance > 1 and key < node.left.key:
            return self.rightRotate(node)

        # Right Right Case
        if balance < -1 and key > node.right.key:
            return self.leftRotate(node)

        # Left Right Case
        if balance > 1 and key > node.left.key:
            node.left = self.leftRotate(node.left)
            return self.rightRotate(node)

        # Right Left Case
        if balance < -1 and key < node.right.key:
            node.right = self.rightRotate(node.right)
            return self.leftRotate(node)

        return node

    def inorder(self, node):
        if node:
            self.inorder(node.left)
            print(node.key)
            self.inorder(node.right)

avl_py = AVLTree()
root_py = None
keys = [10, 20, 30, 40, 50, 25]
for key in keys:
    root_py = avl_py.insert(root_py, key)

print("In-order traversal of AVL Tree (Python):")
avl_py.inorder(root_py)
# Expected: 10, 20, 25, 30, 40, 50 (sorted)
```

### PHP
```php
<?php

class AVLNodePHP {
    public $key;
    public $left;
    public $right;
    public $height;

    public function __construct($key) {
        $this->key = $key;
        $this->left = null;
        $this->right = null;
        $this->height = 1;
    }
}

class AVLTreePHP {
    public $root;

    public function __construct() {
        $this->root = null;
    }

    private function getHeight(?AVLNodePHP $node): int {
        if ($node === null) {
            return 0;
        }
        return $node->height;
    }

    private function getBalance(?AVLNodePHP $node): int {
        if ($node === null) {
            return 0;
        }
        return $this->getHeight($node->left) - $this->getHeight($node->right);
    }

    private function updateHeight(AVLNodePHP $node): void {
        $node->height = 1 + max($this->getHeight($node->left), $this->getHeight($node->right));
    }

    private function rightRotate(AVLNodePHP $y): AVLNodePHP {
        $x = $y->left;
        $T2 = $x->right;

        $x->right = $y;
        $y->left = $T2;

        $this->updateHeight($y);
        $this->updateHeight($x);

        return $x;
    }

    private function leftRotate(AVLNodePHP $x): AVLNodePHP {
        $y = $x->right;
        $T2 = $y->left;

        $y->left = $x;
        $x->right = $T2;

        $this->updateHeight($x);
        $this->updateHeight($y);

        return $y;
    }

    private function insert(?AVLNodePHP $node, $key): AVLNodePHP {
        if ($node === null) {
            return new AVLNodePHP($key);
        }

        if ($key < $node->key) {
            $node->left = $this->insert($node->left, $key);
        } else if ($key > $node->key) {
            $node->right = $this->insert($node->right, $key);
        } else {
            return $node; // Duplicate keys not allowed
        }

        $this->updateHeight($node);

        $balance = $this->getBalance($node);

        // Left Left Case
        if ($balance > 1 && $key < $node->left->key) {
            return $this->rightRotate($node);
        }

        // Right Right Case
        if ($balance < -1 && $key > $node->right->key) {
            return $this->leftRotate($node);
        }

        // Left Right Case
        if ($balance > 1 && $key > $node->left->key) {
            $node->left = $this->leftRotate($node->left);
            return $this->rightRotate($node);
        }

        // Right Left Case
        if ($balance < -1 && $key < $node->right->key) {
            $node->right = $this->rightRotate($node->right);
            return $this->leftRotate($node);
        }

        return $node;
    }

    public function insertKey($key): void {
        $this->root = $this->insert($this->root, $key);
    }

    public function inorder(?AVLNodePHP $node): void {
        if ($node !== null) {
            $this->inorder($node->left);
            echo $node->key . "\n";
            $this->inorder($node->right);
        }
    }
}

$avlPHP = new AVLTreePHP();
$avlPHP->insertKey(10);
$avlPHP->insertKey(20);
$avlPHP->insertKey(30);
$avlPHP->insertKey(40);
$avlPHP->insertKey(50);
$avlPHP->insertKey(25);

echo "In-order traversal of AVL Tree (PHP):\n";
$avlPHP->inorder($avlPHP->root);
// Expected: 10, 20, 25, 30, 40, 50 (sorted)

?>
```