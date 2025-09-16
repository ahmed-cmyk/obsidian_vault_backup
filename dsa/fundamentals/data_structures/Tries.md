# Tries (Prefix Trees)

## Description
A Trie, also known as a prefix tree, is a tree-like data structure used to store a dynamic set or associative array where the keys are usually strings. Unlike a binary search tree, no node in the tree stores the key associated with that node; instead, its position in the tree defines the key with which it is associated. All the descendants of a node have a common prefix of the string associated with that node.

## When to Use
- For efficient retrieval of keys in a dataset, especially when dealing with strings.
- In autocomplete and spell-checker functionalities.
- For dictionary implementations and IP routing.
- When searching for words with a common prefix.

## Time Complexity
- **Insertion:** O(L) (where L is the length of the key)
- **Search:** O(L)
- **Deletion:** O(L)

## Space Complexity
- O(N * M) (where N is the number of keys and M is the average length of keys, can be optimized)

## Implementations

### JavaScript
```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }

    insert(word) {
        let current = this.root;
        for (let i = 0; i < word.length; i++) {
            let char = word[i];
            if (!(char in current.children)) {
                current.children[char] = new TrieNode();
            }
            current = current.children[char];
        }
        current.isEndOfWord = true;
    }

    search(word) {
        let current = this.root;
        for (let i = 0; i < word.length; i++) {
            let char = word[i];
            if (!(char in current.children)) {
                return false;
            }
            current = current.children[char];
        }
        return current.isEndOfWord;
    }

    startsWith(prefix) {
        let current = this.root;
        for (let i = 0; i < prefix.length; i++) {
            let char = prefix[i];
            if (!(char in current.children)) {
                return false;
            }
            current = current.children[char];
        }
        return true;
    }
}

let trieJS = new Trie();
trieJS.insert("apple");
trieJS.insert("app");
trieJS.insert("apricot");

console.log("Search 'apple' (JS):", trieJS.search("apple")); // true
console.log("Search 'app' (JS):", trieJS.search("app"));     // true
console.log("Search 'ap' (JS):", trieJS.search("ap"));       // false
console.log("Starts with 'app' (JS):", trieJS.startsWith("app")); // true
console.log("Starts with 'bat' (JS):", trieJS.startsWith("bat")); // false
```

### TypeScript
```typescript
class TrieNodeTS {
    children: { [key: string]: TrieNodeTS };
    isEndOfWord: boolean;

    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class TrieTS {
    private root: TrieNodeTS;

    constructor() {
        this.root = new TrieNodeTS();
    }

    insert(word: string): void {
        let current = this.root;
        for (let i = 0; i < word.length; i++) {
            let char = word[i];
            if (!(char in current.children)) {
                current.children[char] = new TrieNodeTS();
            }
            current = current.children[char];
        }
        current.isEndOfWord = true;
    }

    search(word: string): boolean {
        let current = this.root;
        for (let i = 0; i < word.length; i++) {
            let char = word[i];
            if (!(char in current.children)) {
                return false;
            }
            current = current.children[char];
        }
        return current.isEndOfWord;
    }

    startsWith(prefix: string): boolean {
        let current = this.root;
        for (let i = 0; i < prefix.length; i++) {
            let char = prefix[i];
            if (!(char in current.children)) {
                return false;
            }
            current = current.children[char];
        }
        return true;
    }
}

let trieTS = new TrieTS();
trieTS.insert("apple");
trieTS.insert("app");
trieTS.insert("apricot");

console.log("Search 'apple' (TS):", trieTS.search("apple")); // true
console.log("Search 'app' (TS):", trieTS.search("app"));     // true
console.log("Search 'ap' (TS):", trieTS.search("ap"));       // false
console.log("Starts with 'app' (TS):", trieTS.startsWith("app")); // true
console.log("Starts with 'bat' (TS):", trieTS.startsWith("bat")); // false
```

### Python
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        current = self.root
        for char in word:
            if char not in current.children:
                current.children[char] = TrieNode()
            current = current.children[char]
        current.is_end_of_word = True

    def search(self, word):
        current = self.root
        for char in word:
            if char not in current.children:
                return False
            current = current.children[char]
        return current.is_end_of_word

    def starts_with(self, prefix):
        current = self.root
        for char in prefix:
            if char not in current.children:
                return False
            current = current.children[char]
        return True

trie_py = Trie()
trie_py.insert("apple")
trie_py.insert("app")
trie_py.insert("apricot")

print("Search 'apple' (Python):", trie_py.search("apple")) # True
print("Search 'app' (Python):", trie_py.search("app"))     # True
print("Search 'ap' (Python):", trie_py.search("ap"))       # False
print("Starts with 'app' (Python):", trie_py.starts_with("app")) # True
print("Starts with 'bat' (Python):", trie_py.starts_with("bat")) # False
```

### PHP
```php
<?php

class TrieNodePHP {
    public $children = [];
    public $isEndOfWord = false;
}

class TriePHP {
    private $root;

    public function __construct() {
        $this->root = new TrieNodePHP();
    }

    public function insert(string $word): void {
        $current = $this->root;
        for ($i = 0; $i < strlen($word); $i++) {
            $char = $word[$i];
            if (!isset($current->children[$char])) {
                $current->children[$char] = new TrieNodePHP();
            }
            $current = $current->children[$char];
        }
        $current->isEndOfWord = true;
    }

    public function search(string $word): bool {
        $current = $this->root;
        for ($i = 0; $i < strlen($word); $i++) {
            $char = $word[$i];
            if (!isset($current->children[$char])) {
                return false;
            }
            $current = $current->children[$char];
        }
        return true;
    }

    public function startsWith(string $prefix): bool {
        $current = $this->root;
        for ($i = 0; $i < strlen($prefix); $i++) {
            $char = $prefix[$i];
            if (!isset($current->children[$char])) {
                return false;
            }
            $current = $current->children[$char];
        }
        return true;
    }
}

$triePHP = new TriePHP();
$triePHP->insert("apple");
$triePHP->insert("app");
$triePHP->insert("apricot");

echo "Search 'apple' (PHP): " . ($triePHP->search("apple") ? "true" : "false") . "\n"; // true
echo "Search 'app' (PHP): " . ($triePHP->search("app") ? "true" : "false") . "\n";     // true
echo "Search 'ap' (PHP): " . ($triePHP->search("ap") ? "true" : "false") . "\n";       // false
echo "Starts with 'app' (PHP): " . ($triePHP->startsWith("app") ? "true" : "false") . "\n"; // true
echo "Starts with 'bat' (PHP): " . ($triePHP->startsWith("bat") ? "true" : "false") . "\n"; // false

?>
```