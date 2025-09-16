# Floyd-Warshall Algorithm

## Description
The Floyd-Warshall algorithm is an algorithm for finding shortest paths in a directed weighted graph with positive or negative edge weights (but no negative cycles). It computes shortest paths between all pairs of nodes. It works by iteratively improving an estimate of the shortest path between two vertices, until the estimate is optimal.

## When to Use
- When you need to find the shortest paths between **all pairs** of vertices in a graph.
- When the graph may contain negative edge weights, but no negative cycles.
- In applications like routing in computer networks, where all-pairs shortest paths are needed.
- For finding the transitive closure of a graph.

## Time Complexity
- O(V^3) (where V is the number of vertices)

## Space Complexity
- O(V^2)

## Implementations

### JavaScript
```javascript
function floydWarshall(graph) {
    const V = Object.keys(graph).length;
    const nodes = Object.keys(graph);
    const dist = {};

    // Initialize distances
    for (let i = 0; i < V; i++) {
        dist[nodes[i]] = {};
        for (let j = 0; j < V; j++) {
            if (nodes[i] === nodes[j]) {
                dist[nodes[i]][nodes[j]] = 0;
            } else if (graph[nodes[i]] && graph[nodes[i]][nodes[j]] !== undefined) {
                dist[nodes[i]][nodes[j]] = graph[nodes[i]][nodes[j]];
            } else {
                dist[nodes[i]][nodes[j]] = Infinity;
            }
        }
    }

    // Main Floyd-Warshall logic
    for (let k = 0; k < V; k++) { // Intermediate vertex
        for (let i = 0; i < V; i++) { // Source vertex
            for (let j = 0; j < V; j++) { // Destination vertex
                if (dist[nodes[i]][nodes[k]] !== Infinity &&
                    dist[nodes[k]][nodes[j]] !== Infinity &&
                    dist[nodes[i]][nodes[k]] + dist[nodes[k]][nodes[j]] < dist[nodes[i]][nodes[j]]) {
                    dist[nodes[i]][nodes[j]] = dist[nodes[i]][nodes[k]] + dist[nodes[k]][nodes[j]];
                }
            }
        }
    }

    // Check for negative cycles (if dist[i][i] < 0)
    for (let i = 0; i < V; i++) {
        if (dist[nodes[i]][nodes[i]] < 0) {
            console.error("Graph contains a negative cycle!");
            return null;
        }
    }

    return dist;
}

const graphFWJS = {
    A: { B: 3, C: 8, E: -4 },
    B: { D: 1, E: 7 },
    C: { B: 4 },
    D: { A: 2, C: -5 },
    E: { D: 6 }
};

console.log("Floyd-Warshall (JS):", floydWarshall(graphFWJS));
/* Expected:
{
  A: { A: 0, B: 1, C: -3, D: -4, E: -4 },
  B: { A: 3, B: 0, C: -4, D: 1, E: -1 },
  C: { A: 6, B: 3, C: 0, D: 4, E: 2 },
  D: { A: 2, B: -1, C: -5, D: 0, E: -2 },
  E: { A: 8, B: 5, C: 1, D: 6, E: 0 }
}
*/
```

### TypeScript
```typescript
interface GraphFWAdjListTS {
    [key: string]: { [neighbor: string]: number };
}

function floydWarshallTS(graph: GraphFWAdjListTS): { [node: string]: { [node: string]: number } } | null {
    const V = Object.keys(graph).length;
    const nodes = Object.keys(graph);
    const dist: { [node: string]: { [node: string]: number } } = {};

    // Initialize distances
    for (let i = 0; i < V; i++) {
        dist[nodes[i]] = {};
        for (let j = 0; j < V; j++) {
            if (nodes[i] === nodes[j]) {
                dist[nodes[i]][nodes[j]] = 0;
            } else if (graph[nodes[i]] && graph[nodes[i]][nodes[j]] !== undefined) {
                dist[nodes[i]][nodes[j]] = graph[nodes[i]][nodes[j]];
            } else {
                dist[nodes[i]][nodes[j]] = Infinity;
            }
        }
    }

    // Main Floyd-Warshall logic
    for (let k = 0; k < V; k++) { // Intermediate vertex
        for (let i = 0; i < V; i++) { // Source vertex
            for (let j = 0; j < V; j++) { // Destination vertex
                if (dist[nodes[i]][nodes[k]] !== Infinity &&
                    dist[nodes[k]][nodes[j]] !== Infinity &&
                    dist[nodes[i]][nodes[k]] + dist[nodes[k]][nodes[j]] < dist[nodes[i]][nodes[j]]) {
                    dist[nodes[i]][nodes[j]] = dist[nodes[i]][nodes[k]] + dist[nodes[k]][nodes[j]];
                }
            }
        }
    }

    // Check for negative cycles (if dist[i][i] < 0)
    for (let i = 0; i < V; i++) {
        if (dist[nodes[i]][nodes[i]] < 0) {
            console.error("Graph contains a negative cycle!");
            return null;
        }
    }

    return dist;
}

const graphFWTS: GraphFWAdjListTS = {
    A: { B: 3, C: 8, E: -4 },
    B: { D: 1, E: 7 },
    C: { B: 4 },
    D: { A: 2, C: -5 },
    E: { D: 6 }
};

console.log("Floyd-Warshall (TS):", floydWarshallTS(graphFWTS));
/* Expected:
{
  A: { A: 0, B: 1, C: -3, D: -4, E: -4 },
  B: { A: 3, B: 0, C: -4, D: 1, E: -1 },
  C: { A: 6, B: 3, C: 0, D: 4, E: 2 },
  D: { A: 2, B: -1, C: -5, D: 0, E: -2 },
  E: { A: 8, B: 5, C: 1, D: 6, E: 0 }
}
*/
```

### Python
```python
def floyd_warshall(graph):
    nodes = list(graph.keys())
    V = len(nodes)
    node_to_idx = {node: i for i, node in enumerate(nodes)}
    idx_to_node = {i: node for i, node in enumerate(nodes)}

    dist = [[float('inf')] * V for _ in range(V)]

    # Initialize distances
    for i in range(V):
        dist[i][i] = 0

    for u_node in graph:
        for v_node, weight in graph[u_node].items():
            u_idx = node_to_idx[u_node]
            v_idx = node_to_idx[v_node]
            dist[u_idx][v_idx] = weight

    # Main Floyd-Warshall logic
    for k in range(V): # Intermediate vertex
        for i in range(V): # Source vertex
            for j in range(V): # Destination vertex
                if dist[i][k] != float('inf') and \
                   dist[k][j] != float('inf') and \
                   dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # Check for negative cycles
    for i in range(V):
        if dist[i][i] < 0:
            print("Graph contains a negative cycle!")
            return None

    # Convert back to node names for readability
    result = {}
    for i in range(V):
        result[idx_to_node[i]] = {}
        for j in range(V):
            result[idx_to_node[i]][idx_to_node[j]] = dist[i][j]

    return result

graph_fw_py = {
    'A': {'B': 3, 'C': 8, 'E': -4},
    'B': {'D': 1, 'E': 7},
    'C': {'B': 4},
    'D': {'A': 2, 'C': -5},
    'E': {'D': 6}
}

print("Floyd-Warshall (Python):", floyd_warshall(graph_fw_py))
# Expected:
# {'A': {'A': 0, 'B': 1, 'C': -3, 'D': -4, 'E': -4}, 
#  'B': {'A': 3, 'B': 0, 'C': -4, 'D': 1, 'E': -1}, 
#  'C': {'A': 6, 'B': 3, 'C': 0, 'D': 4, 'E': 2}, 
#  'D': {'A': 2, 'B': -1, 'C': -5, 'D': 0, 'E': -2}, 
#  'E': {'A': 8, 'B': 5, 'C': 1, 'D': 6, 'E': 0}}
```

### PHP
```php
<?php

function floydWarshallPHP(array $graph): ?array {
    $nodes = array_keys($graph);
    $V = count($nodes);
    $nodeToIndex = array_flip($nodes);
    $indexToNode = $nodes;

    $dist = [];
    for ($i = 0; $i < $V; $i++) {
        $dist[$i] = array_fill(0, $V, INF);
        $dist[$i][$i] = 0;
    }

    // Initialize distances from graph
    foreach ($graph as $uNode => $edges) {
        $uIdx = $nodeToIndex[$uNode];
        foreach ($edges as $vNode => $weight) {
            $vIdx = $nodeToIndex[$vNode];
            $dist[$uIdx][$vIdx] = $weight;
        }
    }

    // Main Floyd-Warshall logic
    for ($k = 0; $k < $V; $k++) { // Intermediate vertex
        for ($i = 0; $i < $V; $i++) { // Source vertex
            for ($j = 0; $j < $V; $j++) { // Destination vertex
                if ($dist[$i][$k] !== INF &&
                    $dist[$k][$j] !== INF &&
                    $dist[$i][$k] + $dist[$k][$j] < $dist[$i][$j]) {
                    $dist[$i][$j] = $dist[$i][$k] + $dist[$k][$j];
                }
            }
        }
    }

    // Check for negative cycles
    for ($i = 0; $i < $V; $i++) {
        if ($dist[$i][$i] < 0) {
            echo "Graph contains a negative cycle!\n";
            return null;
        }
    }

    // Convert back to node names for readability
    $result = [];
    for ($i = 0; $i < $V; $i++) {
        $result[$indexToNode[$i]] = [];
        for ($j = 0; $j < $V; $j++) {
            $result[$indexToNode[$i]][$indexToNode[$j]] = $dist[$i][$j];
        }
    }

    return $result;
}

$graphFWPHP = [
    'A' => ['B' => 3, 'C' => 8, 'E' => -4],
    'B' => ['D' => 1, 'E' => 7],
    'C' => ['B' => 4],
    'D' => ['A' => 2, 'C' => -5],
    'E' => ['D' => 6]
];

echo "Floyd-Warshall (PHP):\n";
print_r(floydWarshallPHP($graphFWPHP));
/* Expected:
Array
(
    [A] => Array
        (
            [A] => 0
            [B] => 1
            [C] => -3
            [D] => -4
            [E] => -4
        )

    [B] => Array
        (
            [A] => 3
            [B] => 0
            [C] => -4
            [D] => 1
            [E] => -1
        )

    [C] => Array
        (
            [A] => 6
            [B] => 3
            [C] => 0
            [D] => 4
            [E] => 2
        )

    [D] => Array
        (
            [A] => 2
            [B] => -1
            [C] => -5
            [D] => 0
            [E] => -2
        )

    [E] => Array
        (
            [A] => 8
            [B] => 5
            [C] => 1
            [D] => 6
            [E] => 0
        )

)
*/

?>
```
```