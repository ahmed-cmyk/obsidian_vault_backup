# Bellman-Ford Algorithm

## Description
Bellman-Ford algorithm is a single-source shortest path algorithm that finds the shortest paths from a single source vertex to all other vertices in a weighted digraph. It is more versatile than Dijkstra's algorithm because it can handle graphs in which some of the edge weights are negative. However, it is slower than Dijkstra's algorithm. The algorithm works by repeatedly relaxing edges, meaning it iteratively updates the shortest path estimates until no more improvements can be made. It can also detect negative cycles.

## When to Use
- When finding the shortest path from a single source to all other vertices in a graph that may contain negative edge weights.
- For detecting negative cycles in a graph.
- In network routing protocols, such as the Distance Vector Routing Protocol.

## Time Complexity
- O(V * E) (where V is the number of vertices and E is the number of edges)

## Space Complexity
- O(V)

## Implementations

### JavaScript
```javascript
function bellmanFord(graph, startNode) {
    const distances = {};
    const predecessors = {};
    const vertices = Object.keys(graph);

    // Step 1: Initialize distances and predecessors
    for (let vertex of vertices) {
        distances[vertex] = Infinity;
        predecessors[vertex] = null;
    }
    distances[startNode] = 0;

    // Step 2: Relax edges |V| - 1 times
    for (let i = 0; i < vertices.length - 1; i++) {
        for (let u of vertices) {
            for (let v in graph[u]) {
                let weight = graph[u][v];
                if (distances[u] !== Infinity && distances[u] + weight < distances[v]) {
                    distances[v] = distances[u] + weight;
                    predecessors[v] = u;
                }
            }
        }
    }

    // Step 3: Check for negative cycles
    for (let u of vertices) {
        for (let v in graph[u]) {
            let weight = graph[u][v];
            if (distances[u] !== Infinity && distances[u] + weight < distances[v]) {
                console.error("Graph contains a negative cycle!");
                return null; // Negative cycle detected
            }
        }
    }

    return { distances, predecessors };
}

const graphBFJS = {
    A: { B: -1, C: 4 },
    B: { C: 3, D: 2, E: 2 },
    C: {},
    D: { B: 1, C: 5 },
    E: { D: -3 }
};

console.log("Bellman-Ford (JS) from A:", bellmanFord(graphBFJS, "A"));
/* Expected:
{
  distances: { A: 0, B: -1, C: 2, D: -2, E: 1 },
  predecessors: { A: null, B: 'A', C: 'B', D: 'E', E: 'B' }
}
*/

const graphBFJS_neg_cycle = {
    A: { B: 1 },
    B: { C: -1 },
    C: { A: -1 }
};
console.log("Bellman-Ford (JS) with negative cycle from A:", bellmanFord(graphBFJS_neg_cycle, "A"));
// Expected: Graph contains a negative cycle! null
```

### TypeScript
```typescript
interface GraphBFAdjListTS {
    [key: string]: { [neighbor: string]: number };
}

function bellmanFordTS(graph: GraphBFAdjListTS, startNode: string): { distances: { [node: string]: number }, predecessors: { [node: string]: string | null } } | null {
    const distances: { [node: string]: number } = {};
    const predecessors: { [node: string]: string | null } = {};
    const vertices = Object.keys(graph);

    // Step 1: Initialize distances and predecessors
    for (let vertex of vertices) {
        distances[vertex] = Infinity;
        predecessors[vertex] = null;
    }
    distances[startNode] = 0;

    // Step 2: Relax edges |V| - 1 times
    for (let i = 0; i < vertices.length - 1; i++) {
        for (let u of vertices) {
            for (let v in graph[u]) {
                let weight = graph[u][v];
                if (distances[u] !== Infinity && distances[u] + weight < distances[v]) {
                    distances[v] = distances[u] + weight;
                    predecessors[v] = u;
                }
            }
        }
    }

    // Step 3: Check for negative cycles
    for (let u of vertices) {
        for (let v in graph[u]) {
            let weight = graph[u][v];
            if (distances[u] !== Infinity && distances[u] + weight < distances[v]) {
                console.error("Graph contains a negative cycle!");
                return null; // Negative cycle detected
            }
        }
    }

    return { distances, predecessors };
}

const graphBFTS: GraphBFAdjListTS = {
    A: { B: -1, C: 4 },
    B: { C: 3, D: 2, E: 2 },
    C: {},
    D: { B: 1, C: 5 },
    E: { D: -3 }
};

console.log("Bellman-Ford (TS) from A:", bellmanFordTS(graphBFTS, "A"));
/* Expected:
{
  distances: { A: 0, B: -1, C: 2, D: -2, E: 1 },
  predecessors: { A: null, B: 'A', C: 'B', D: 'E', E: 'B' }
}
*/

const graphBFTS_neg_cycle: GraphBFAdjListTS = {
    A: { B: 1 },
    B: { C: -1 },
    C: { A: -1 }
};
console.log("Bellman-Ford (TS) with negative cycle from A:", bellmanFordTS(graphBFTS_neg_cycle, "A"));
// Expected: Graph contains a negative cycle! null
```

### Python
```python
def bellman_ford(graph, start_node):
    distances = {vertex: float('inf') for vertex in graph}
    predecessors = {vertex: None for vertex in graph}
    distances[start_node] = 0

    vertices = list(graph.keys())
    num_vertices = len(vertices)

    # Step 2: Relax edges |V| - 1 times
    for _ in range(num_vertices - 1):
        for u in vertices:
            for v, weight in graph[u].items():
                if distances[u] != float('inf') and distances[u] + weight < distances[v]:
                    distances[v] = distances[u] + weight
                    predecessors[v] = u

    # Step 3: Check for negative cycles
    for u in vertices:
        for v, weight in graph[u].items():
            if distances[u] != float('inf') and distances[u] + weight < distances[v]:
                print("Graph contains a negative cycle!")
                return None # Negative cycle detected

    return {'distances': distances, 'predecessors': predecessors}

graph_bf_py = {
    'A': {'B': -1, 'C': 4},
    'B': {'C': 3, 'D': 2, 'E': 2},
    'C': {},
    'D': {'B': 1, 'C': 5},
    'E': {'D': -3}
}

print("Bellman-Ford (Python) from A:", bellman_ford(graph_bf_py, 'A'))
# Expected: {'distances': {'A': 0, 'B': -1, 'C': 2, 'D': -2, 'E': 1}, 'predecessors': {'A': None, 'B': 'A', 'C': 'B', 'D': 'E', 'E': 'B'}}

graph_bf_py_neg_cycle = {
    'A': {'B': 1},
    'B': {'C': -1},
    'C': {'A': -1}
}
print("Bellman-Ford (Python) with negative cycle from A:", bellman_ford(graph_bf_py_neg_cycle, 'A'))
# Expected: Graph contains a negative cycle! None
```

### PHP
```php
<?php

function bellmanFordPHP(array $graph, string $startNode): ?array {
    $distances = [];
    $predecessors = [];
    $vertices = array_keys($graph);
    $numVertices = count($vertices);

    // Step 1: Initialize distances and predecessors
    foreach ($vertices as $vertex) {
        $distances[$vertex] = INF;
        $predecessors[$vertex] = null;
    }
    $distances[$startNode] = 0;

    // Step 2: Relax edges |V| - 1 times
    for ($i = 0; $i < $numVertices - 1; $i++) {
        foreach ($vertices as $u) {
            if ($distances[$u] === INF) continue;
            foreach ($graph[$u] as $v => $weight) {
                if ($distances[$u] + $weight < $distances[$v]) {
                    $distances[$v] = $distances[$u] + $weight;
                    $predecessors[$v] = $u;
                }
            }
        }
    }

    // Step 3: Check for negative cycles
    foreach ($vertices as $u) {
        if ($distances[$u] === INF) continue;
        foreach ($graph[$u] as $v => $weight) {
            if ($distances[$u] + $weight < $distances[$v]) {
                echo "Graph contains a negative cycle!\n";
                return null; // Negative cycle detected
            }
        }
    }

    return ['distances' => $distances, 'predecessors' => $predecessors];
}

$graphBFPHP = [
    'A' => ['B' => -1, 'C' => 4],
    'B' => ['C' => 3, 'D' => 2, 'E' => 2],
    'C' => [],
    'D' => ['B' => 1, 'C' => 5],
    'E' => ['D' => -3]
];

echo "Bellman-Ford (PHP) from A:\n";
print_r(bellmanFordPHP($graphBFPHP, 'A'));
/* Expected:
Array
(
    [distances] => Array
        (
            [A] => 0
            [B] => -1
            [C] => 2
            [D] => -2
            [E] => 1
        )

    [predecessors] => Array
        (
            [A] => 
            [B] => A
            [C] => B
            [D] => E
            [E] => B
        )

)
*/

$graphBFPHP_neg_cycle = [
    'A' => ['B' => 1],
    'B' => ['C' => -1],
    'C' => ['A' => -1]
];
echo "Bellman-Ford (PHP) with negative cycle from A:\n";
print_r(bellmanFordPHP($graphBFPHP_neg_cycle, 'A'));
// Expected: Graph contains a negative cycle! null

?>
```
```