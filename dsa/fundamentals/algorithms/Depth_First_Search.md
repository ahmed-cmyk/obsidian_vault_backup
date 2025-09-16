# Depth-First Search (DFS)

## Description
Depth-First Search (DFS) is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at the root (or an arbitrary node) and explores as far as possible along each branch before backtracking. It uses a stack (implicitly via recursion or explicitly) to remember the next vertex to visit.

## When to Use
- For finding connected components in a graph.
- For topological sorting of a directed acyclic graph (DAG).
- For detecting cycles in a graph.
- In pathfinding problems where any path is acceptable (e.g., maze solving).
- For exploring all nodes and edges of a graph or tree.

## Time Complexity
- O(V + E) (where V is the number of vertices and E is the number of edges, for both graphs and trees)

## Space Complexity
- O(V) (due to the recursion stack or explicit stack for storing visited nodes)

## Implementations (Graph Traversal)

### JavaScript
```javascript
class GraphDFS {
    constructor() {
        this.adjacencyList = {};
    }

    addVertex(vertex) {
        if (!this.adjacencyList[vertex]) {
            this.adjacencyList[vertex] = [];
        }
    }

    addEdge(vertex1, vertex2) {
        this.adjacencyList[vertex1].push(vertex2);
        this.adjacencyList[vertex2].push(vertex1); // For undirected graph
    }

    dfsRecursive(start) {
        const result = [];
        const visited = {};
        const adjacencyList = this.adjacencyList;

        (function dfs(vertex) {
            visited[vertex] = true;
            result.push(vertex);

            adjacencyList[vertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    return dfs(neighbor);
                }
            });
        })(start);

        return result;
    }

    dfsIterative(start) {
        const stack = [start];
        const result = [];
        const visited = {};
        visited[start] = true;
        let currentVertex;

        while (stack.length) {
            currentVertex = stack.pop();
            result.push(currentVertex);

            this.adjacencyList[currentVertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    stack.push(neighbor);
                }
            });
        }
        return result;
    }
}

let graphDFSJS = new GraphDFS();
graphDFSJS.addVertex("A");
graphDFSJS.addVertex("B");
graphDFSJS.addVertex("C");
graphDFSJS.addVertex("D");
graphDFSJS.addVertex("E");
graphDFSJS.addVertex("F");

graphDFSJS.addEdge("A", "B");
graphDFSJS.addEdge("A", "C");
graphDFSJS.addEdge("B", "D");
graphDFSJS.addEdge("C", "E");
graphDFSJS.addEdge("D", "E");
graphDFSJS.addEdge("D", "F");
graphDFSJS.addEdge("E", "F");

console.log("DFS Recursive (JS):", graphDFSJS.dfsRecursive("A")); // [ 'A', 'B', 'D', 'E', 'C', 'F' ] (order may vary)
console.log("DFS Iterative (JS):", graphDFSJS.dfsIterative("A")); // [ 'A', 'C', 'E', 'F', 'D', 'B' ] (order may vary)
```

### TypeScript
```typescript
interface AdjacencyListDFS<T> {
    [key: string]: T[];
}

class GraphDFS_TS<T> {
    private adjacencyList: AdjacencyListDFS<T>;

    constructor() {
        this.adjacencyList = {};
    }

    addVertex(vertex: T): void {
        if (!this.adjacencyList[String(vertex)]) {
            this.adjacencyList[String(vertex)] = [];
        }
    }

    addEdge(vertex1: T, vertex2: T): void {
        this.adjacencyList[String(vertex1)].push(vertex2);
        this.adjacencyList[String(vertex2)].push(vertex1); // For undirected graph
    }

    dfsRecursive(start: T): T[] {
        const result: T[] = [];
        const visited: { [key: string]: boolean } = {};
        const adjacencyList = this.adjacencyList;

        const dfs = (vertex: T) => {
            visited[String(vertex)] = true;
            result.push(vertex);

            adjacencyList[String(vertex)].forEach(neighbor => {
                if (!visited[String(neighbor)]) {
                    dfs(neighbor);
                }
            });
        };

        dfs(start);
        return result;
    }

    dfsIterative(start: T): T[] {
        const stack: T[] = [start];
        const result: T[] = [];
        const visited: { [key: string]: boolean } = {};
        visited[String(start)] = true;
        let currentVertex: T;

        while (stack.length) {
            currentVertex = stack.pop()!;
            result.push(currentVertex);

            this.adjacencyList[String(currentVertex)].forEach(neighbor => {
                if (!visited[String(neighbor)]) {
                    visited[String(neighbor)] = true;
                    stack.push(neighbor);
                }
            });
        }
        return result;
    }
}

let graphDFS_TS = new GraphDFS_TS<string>();
graphDFS_TS.addVertex("A");
graphDFS_TS.addVertex("B");
graphDFS_TS.addVertex("C");
graphDFS_TS.addVertex("D");
graphDFS_TS.addVertex("E");
graphDFS_TS.addVertex("F");

graphDFS_TS.addEdge("A", "B");
graphDFS_TS.addEdge("A", "C");
graphDFS_TS.addEdge("B", "D");
graphDFS_TS.addEdge("C", "E");
graphDFS_TS.addEdge("D", "E");
graphDFS_TS.addEdge("D", "F");
graphDFS_TS.addEdge("E", "F");

console.log("DFS Recursive (TS):", graphDFS_TS.dfsRecursive("A")); // [ 'A', 'B', 'D', 'E', 'C', 'F' ] (order may vary)
console.log("DFS Iterative (TS):", graphDFS_TS.dfsIterative("A")); // [ 'A', 'C', 'E', 'F', 'D', 'B' ] (order may vary)
```

### Python
```python
class GraphDFS_Py:
    def __init__(self):
        self.adjacency_list = {}

    def add_vertex(self, vertex):
        if vertex not in self.adjacency_list:
            self.adjacency_list[vertex] = []

    def add_edge(self, vertex1, vertex2):
        self.adjacency_list[vertex1].append(vertex2)
        self.adjacency_list[vertex2].append(vertex1) # For undirected graph

    def dfs_recursive(self, start):
        result = []
        visited = {}

        def dfs(vertex):
            visited[vertex] = True
            result.append(vertex)

            for neighbor in self.adjacency_list[vertex]:
                if neighbor not in visited:
                    dfs(neighbor)

        dfs(start)
        return result

    def dfs_iterative(self, start):
        stack = [start]
        result = []
        visited = {start: True}

        while stack:
            current_vertex = stack.pop()
            result.append(current_vertex)

            for neighbor in self.adjacency_list[current_vertex]:
                if neighbor not in visited:
                    visited[neighbor] = True
                    stack.append(neighbor)
        return result

graphDFS_Py = GraphDFS_Py()
graphDFS_Py.add_vertex("A")
graphDFS_Py.add_vertex("B")
graphDFS_Py.add_vertex("C")
graphDFS_Py.add_vertex("D")
graphDFS_Py.add_vertex("E")
graphDFS_Py.add_vertex("F")

graphDFS_Py.add_edge("A", "B")
graphDFS_Py.add_edge("A", "C")
graphDFS_Py.add_edge("B", "D")
graphDFS_Py.add_edge("C", "E")
graphDFS_Py.add_edge("D", "E")
graphDFS_Py.add_edge("D", "F")
graphDFS_Py.add_edge("E", "F")

print("DFS Recursive (Python):", graphDFS_Py.dfs_recursive("A")) # ['A', 'B', 'D', 'E', 'C', 'F'] (order may vary)
print("DFS Iterative (Python):", graphDFS_Py.dfs_iterative("A")) # ['A', 'C', 'E', 'F', 'D', 'B'] (order may vary)
```

### PHP
```php
<?php

class GraphDFS_PHP {
    private $adjacencyList = [];

    public function addVertex($vertex) {
        if (!isset($this->adjacencyList[$vertex])) {
            $this->adjacencyList[$vertex] = [];
        }
    }

    public function addEdge($vertex1, $vertex2) {
        $this->adjacencyList[$vertex1][] = $vertex2;
        $this->adjacencyList[$vertex2][] = $vertex1; // For undirected graph
    }

    public function dfsRecursive($start) {
        $result = [];
        $visited = [];

        $dfs = function($vertex) use (&$result, &$visited, &$dfs) {
            $visited[$vertex] = true;
            $result[] = $vertex;

            foreach ($this->adjacencyList[$vertex] as $neighbor) {
                if (!isset($visited[$neighbor])) {
                    $dfs($neighbor);
                }
            }
        };

        $dfs($start);
        return $result;
    }

    public function dfsIterative($start) {
        $stack = [$start];
        $result = [];
        $visited = [$start => true];

        while (!empty($stack)) {
            $currentVertex = array_pop($stack);
            $result[] = $currentVertex;

            foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
                if (!isset($visited[$neighbor])) {
                    $visited[$neighbor] = true;
                    array_push($stack, $neighbor);
                }
            }
        }
        return $result;
    }
}

$graphDFS_PHP = new GraphDFS_PHP();
$graphDFS_PHP->addVertex("A");
$graphDFS_PHP->addVertex("B");
$graphDFS_PHP->addVertex("C");
$graphDFS_PHP->addVertex("D");
$graphDFS_PHP->addVertex("E");
$graphDFS_PHP->addVertex("F");

$graphDFS_PHP->addEdge("A", "B");
$graphDFS_PHP->addEdge("A", "C");
$graphDFS_PHP->addEdge("B", "D");
$graphDFS_PHP->addEdge("C", "E");
$graphDFS_PHP->addEdge("D", "E");
$graphDFS_PHP->addEdge("D", "F");
graphDFS_PHP->addEdge("E", "F");

echo "DFS Recursive (PHP): " . implode(", ", $graphDFS_PHP->dfsRecursive("A")) . "\n"; // A, B, D, E, C, F (order may vary)
echo "DFS Iterative (PHP): " . implode(", ", $graphDFS_PHP->dfsIterative("A")) . "\n"; // A, C, E, F, D, B (order may vary)

?>
```
```