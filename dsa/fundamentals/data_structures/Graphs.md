# Graphs

## Description
A graph is a non-linear data structure consisting of a finite set of vertices (or nodes) and a set of edges (or links) that connect pairs of vertices. Graphs can be directed (edges have a direction) or undirected (edges have no direction), and weighted (edges have a cost) or unweighted.

## When to Use
- When modeling relationships between entities, such as social networks, road maps, or computer networks.
- For pathfinding algorithms (e.g., shortest path, minimum spanning tree).
- In recommendation systems, dependency tracking, and state machines.
- For representing flow networks and circuit designs.

## Time Complexity (Common Operations - Adjacency List Representation)
- **Adding a Vertex:** O(1)
- **Adding an Edge:** O(1)
- **Checking if two vertices are connected:** O(degree of vertex)
- **Traversals (BFS/DFS):** O(V + E) where V is the number of vertices and E is the number of edges.

## Space Complexity
- O(V + E) (for adjacency list representation)

## Implementations (Adjacency List)

### JavaScript
```javascript
class Graph {
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

    removeEdge(vertex1, vertex2) {
        this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
            v => v !== vertex2
        );
        this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
            v => v !== vertex1
        );
    }

    removeVertex(vertex) {
        while (this.adjacencyList[vertex].length) {
            const adjacentVertex = this.adjacencyList[vertex].pop();
            this.removeEdge(vertex, adjacentVertex);
        }
        delete this.adjacencyList[vertex];
    }

    printGraph() {
        for (let vertex in this.adjacencyList) {
            console.log(`${vertex} -> ${this.adjacencyList[vertex].join(", ")}`);
        }
    }
}

let graphJS = new Graph();
graphJS.addVertex("A");
graphJS.addVertex("B");
graphJS.addVertex("C");
graphJS.addVertex("D");

graphJS.addEdge("A", "B");
graphJS.addEdge("A", "C");
graphJS.addEdge("B", "D");
graphJS.addEdge("C", "D");

console.log("Graph (JS):");
graphJS.printGraph();
// Output:
// A -> B, C
// B -> A, D
// C -> A, D
// D -> B, C
```

### TypeScript
```typescript
interface AdjacencyList<T> {
    [key: string]: T[];
}

class GraphTS<T> {
    private adjacencyList: AdjacencyList<T>;

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

    removeEdge(vertex1: T, vertex2: T): void {
        this.adjacencyList[String(vertex1)] = this.adjacencyList[String(vertex1)].filter(
            v => v !== vertex2
        );
        this.adjacencyList[String(vertex2)] = this.adjacencyList[String(vertex2)].filter(
            v => v !== vertex1
        );
    }

    removeVertex(vertex: T): void {
        while (this.adjacencyList[String(vertex)].length) {
            const adjacentVertex = this.adjacencyList[String(vertex)].pop();
            this.removeEdge(vertex, adjacentVertex!);
        }
        delete this.adjacencyList[String(vertex)];
    }

    printGraph(): void {
        for (let vertex in this.adjacencyList) {
            console.log(`${vertex} -> ${this.adjacencyList[vertex].join(", ")}`);
        }
    }
}

let graphTS = new GraphTS<string>();
graphTS.addVertex("A");
graphTS.addVertex("B");
graphTS.addVertex("C");
graphTS.addVertex("D");

graphTS.addEdge("A", "B");
graphTS.addEdge("A", "C");
graphTS.addEdge("B", "D");
graphTS.addEdge("C", "D");

console.log("Graph (TS):");
graphTS.printGraph();
// Output:
// A -> B, C
// B -> A, D
// C -> A, D
// D -> B, C
```

### Python
```python
class Graph:
    def __init__(self):
        self.adjacency_list = {}

    def add_vertex(self, vertex):
        if vertex not in self.adjacency_list:
            self.adjacency_list[vertex] = []

    def add_edge(self, vertex1, vertex2):
        self.adjacency_list[vertex1].append(vertex2)
        self.adjacency_list[vertex2].append(vertex1) # For undirected graph

    def remove_edge(self, vertex1, vertex2):
        self.adjacency_list[vertex1] = [v for v in self.adjacency_list[vertex1] if v != vertex2]
        self.adjacency_list[vertex2] = [v for v in self.adjacency_list[vertex2] if v != vertex1]

    def remove_vertex(self, vertex):
        while self.adjacency_list[vertex]:
            adjacent_vertex = self.adjacency_list[vertex].pop()
            self.remove_edge(vertex, adjacent_vertex)
        del self.adjacency_list[vertex]

    def print_graph(self):
        for vertex in self.adjacency_list:
            print(f"{vertex} -> {', '.join(map(str, self.adjacency_list[vertex]))}")

graph_py = Graph()
graph_py.add_vertex("A")
graph_py.add_vertex("B")
graph_py.add_vertex("C")
graph_py.add_vertex("D")

graph_py.add_edge("A", "B")
graph_py.add_edge("A", "C")
graph_py.add_edge("B", "D")
graph_py.add_edge("C", "D")

print("Graph (Python):")
graph_py.print_graph()
# Output:
# A -> B, C
# B -> A, D
# C -> A, D
# D -> B, C
```

### PHP
```php
<?php

class GraphPHP {
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

    public function removeEdge($vertex1, $vertex2) {
        $this->adjacencyList[$vertex1] = array_values(array_filter($this->adjacencyList[$vertex1], function($v) use ($vertex2) { return $v !== $vertex2; }));
        $this->adjacencyList[$vertex2] = array_values(array_filter($this->adjacencyList[$vertex2], function($v) use ($vertex1) { return $v !== $vertex1; }));
    }

    public function removeVertex($vertex) {
        while (!empty($this->adjacencyList[$vertex])) {
            $adjacentVertex = array_pop($this->adjacencyList[$vertex]);
            $this->removeEdge($vertex, $adjacentVertex);
        }
        unset($this->adjacencyList[$vertex]);
    }

    public function printGraph() {
        foreach ($this->adjacencyList as $vertex => $connections) {
            echo "{$vertex} -> " . implode(", ", $connections) . "\n";
        }
    }
}

$graphPHP = new GraphPHP();
$graphPHP->addVertex("A");
$graphPHP->addVertex("B");
$graphPHP->addVertex("C");
$graphPHP->addVertex("D");

$graphPHP->addEdge("A", "B");
$graphPHP->addEdge("A", "C");
$graphPHP->addEdge("B", "D");
$graphPHP->addEdge("C", "D");

echo "Graph (PHP):\n";
$graphPHP->printGraph();
// Output:
// A -> B, C
// B -> A, D
// C -> A, D
// D -> B, C

?>
```