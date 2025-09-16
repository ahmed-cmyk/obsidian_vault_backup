# Breadth-First Search (BFS)

## Description
Breadth-First Search (BFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root (or some arbitrary node of a graph, sometimes referred to as a 'search key') and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level. It uses a queue to remember the next vertex to visit.

## When to Use
- For finding the shortest path in an unweighted graph.
- For finding all nodes within a connected component.
- In web crawlers to explore pages level by level.
- For peer-to-peer networking.
- In garbage collection algorithms.

## Time Complexity
- O(V + E) (where V is the number of vertices and E is the number of edges, for both graphs and trees)

## Space Complexity
- O(V) (due to the queue for storing nodes at the current level)

## Implementations (Graph Traversal)

### JavaScript
```javascript
class GraphBFS {
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

    bfs(start) {
        const queue = [start];
        const result = [];
        const visited = {};
        visited[start] = true;
        let currentVertex;

        while (queue.length) {
            currentVertex = queue.shift();
            result.push(currentVertex);

            this.adjacencyList[currentVertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.push(neighbor);
                }
            });
        }
        return result;
    }
}

let graphBFSJS = new GraphBFS();
graphBFSJS.addVertex("A");
graphBFSJS.addVertex("B");
graphBFSJS.addVertex("C");
graphBFSJS.addVertex("D");
graphBFSJS.addVertex("E");
graphBFSJS.addVertex("F");

graphBFSJS.addEdge("A", "B");
graphBFSJS.addEdge("A", "C");
graphBFSJS.addEdge("B", "D");
graphBFSJS.addEdge("C", "E");
graphBFSJS.addEdge("D", "E");
graphBFSJS.addEdge("D", "F");
graphBFSJS.addEdge("E", "F");

console.log("BFS (JS):", graphBFSJS.bfs("A")); // [ 'A', 'B', 'C', 'D', 'E', 'F' ] (order may vary)
```

### TypeScript
```typescript
interface AdjacencyListBFS<T> {
    [key: string]: T[];
}

class GraphBFS_TS<T> {
    private adjacencyList: AdjacencyListBFS<T>;

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

    bfs(start: T): T[] {
        const queue: T[] = [start];
        const result: T[] = [];
        const visited: { [key: string]: boolean } = {};
        visited[String(start)] = true;
        let currentVertex: T;

        while (queue.length) {
            currentVertex = queue.shift()!;
            result.push(currentVertex);

            this.adjacencyList[String(currentVertex)].forEach(neighbor => {
                if (!visited[String(neighbor)]) {
                    visited[String(neighbor)] = true;
                    queue.push(neighbor);
                }
            });
        }
        return result;
    }
}

let graphBFS_TS = new GraphBFS_TS<string>();
graphBFS_TS.addVertex("A");
graphBFS_TS.addVertex("B");
graphBFS_TS.addVertex("C");
graphBFS_TS.addVertex("D");
graphBFS_TS.addVertex("E");
graphBFS_TS.addVertex("F");

graphBFS_TS.addEdge("A", "B");
graphBFS_TS.addEdge("A", "C");
graphBFS_TS.addEdge("B", "D");
graphBFS_TS.addEdge("C", "E");
graphBFS_TS.addEdge("D", "E");
graphBFS_TS.addEdge("D", "F");
graphBFS_TS.addEdge("E", "F");

console.log("BFS (TS):", graphBFS_TS.bfs("A")); // [ 'A', 'B', 'C', 'D', 'E', 'F' ] (order may vary)
```

### Python
```python
from collections import deque

class GraphBFS_Py:
    def __init__(self):
        self.adjacency_list = {}

    def add_vertex(self, vertex):
        if vertex not in self.adjacency_list:
            self.adjacency_list[vertex] = []

    def add_edge(self, vertex1, vertex2):
        self.adjacency_list[vertex1].append(vertex2)
        self.adjacency_list[vertex2].append(vertex1) # For undirected graph

    def bfs(self, start):
        queue = deque([start])
        result = []
        visited = {start: True}

        while queue:
            current_vertex = queue.popleft()
            result.append(current_vertex)

            for neighbor in self.adjacency_list[current_vertex]:
                if neighbor not in visited:
                    visited[neighbor] = True
                    queue.append(neighbor)
        return result

graphBFS_Py = GraphBFS_Py()
graphBFS_Py.add_vertex("A")
graphBFS_Py.add_vertex("B")
graphBFS_Py.add_vertex("C")
graphBFS_Py.add_vertex("D")
graphBFS_Py.add_vertex("E")
graphBFS_Py.add_vertex("F")

graphBFS_Py.add_edge("A", "B")
graphBFS_Py.add_edge("A", "C")
graphBFS_Py.add_edge("B", "D")
graphBFS_Py.add_edge("C", "E")
graphBFS_Py.add_edge("D", "E")
graphBFS_Py.add_edge("D", "F")
graphBFS_Py.add_edge("E", "F")

print("BFS (Python):", graphBFS_Py.bfs("A")) # ['A', 'B', 'C', 'D', 'E', 'F'] (order may vary)
```

### PHP
```php
<?php

class GraphBFS_PHP {
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

    public function bfs($start) {
        $queue = new SplQueue();
        $queue->enqueue($start);
        $result = [];
        $visited = [$start => true];

        while (!$queue->isEmpty()) {
            $currentVertex = $queue->dequeue();
            $result[] = $currentVertex;

            foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
                if (!isset($visited[$neighbor])) {
                    $visited[$neighbor] = true;
                    $queue->enqueue($neighbor);
                }
            }
        }
        return $result;
    }
}

$graphBFS_PHP = new GraphBFS_PHP();
$graphBFS_PHP->addVertex("A");
$graphBFS_PHP->addVertex("B");
$graphBFS_PHP->addVertex("C");
$graphBFS_PHP->addVertex("D");
$graphBFS_PHP->addVertex("E");
$graphBFS_PHP->addVertex("F");

$graphBFS_PHP->addEdge("A", "B");
$graphBFS_PHP->addEdge("A", "C");
$graphBFS_PHP->addEdge("B", "D");
$graphBFS_PHP->addEdge("C", "E");
$graphBFS_PHP->addEdge("D", "E");
$graphBFS_PHP->addEdge("D", "F");
graphBFS_PHP.addEdge("E", "F");

echo "BFS (PHP): " . implode(", ", $graphBFS_PHP->bfs("A")) . "\n"; // A, B, C, D, E, F (order may vary)

?>
```