# Topological Sort

## Description
Topological sorting for a Directed Acyclic Graph (DAG) is a linear ordering of its vertices such that for every directed edge `u -> v`, vertex `u` comes before `v` in the ordering. Topological sort is not unique; a DAG can have multiple topological sorts. It's often used for scheduling tasks or resolving dependencies.

## When to Use
- For scheduling tasks with dependencies (e.g., build systems, course prerequisites).
- In dependency resolution for software packages.
- For ordering cells in a spreadsheet that depend on other cells.
- In compilers for instruction scheduling.

## Time Complexity
- O(V + E) (where V is the number of vertices and E is the number of edges)

## Space Complexity
- O(V + E)

## Implementations (Kahn's Algorithm - BFS based)

### JavaScript
```javascript
function topologicalSortKahn(graph) {
    const inDegree = {};
    const queue = [];
    const result = [];

    // Initialize in-degrees and result list
    for (let node in graph) {
        inDegree[node] = 0;
    }

    for (let node in graph) {
        for (let neighbor of graph[node]) {
            inDegree[neighbor]++;
        }
    }

    // Add all nodes with in-degree 0 to the queue
    for (let node in inDegree) {
        if (inDegree[node] === 0) {
            queue.push(node);
        }
    }

    while (queue.length > 0) {
        let u = queue.shift();
        result.push(u);

        for (let v of graph[u]) {
            inDegree[v]--;
            if (inDegree[v] === 0) {
                queue.push(v);
            }
        }
    }

    // Check for cycle
    if (result.length !== Object.keys(graph).length) {
        console.error("Graph contains a cycle!");
        return null;
    }

    return result;
}

const graphTSKahnJS = {
    'A': ['C'],
    'B': ['C', 'D'],
    'C': ['E'],
    'D': ['F'],
    'E': ['F'],
    'F': []
};

console.log("Topological Sort (Kahn's JS):", topologicalSortKahn(graphTSKahnJS));
// Expected: [ 'A', 'B', 'C', 'D', 'E', 'F' ] (or [ 'B', 'A', 'C', 'D', 'E', 'F' ] etc.)

const graphTSKahnJS_cycle = {
    'A': ['B'],
    'B': ['C'],
    'C': ['A']
};
console.log("Topological Sort (Kahn's JS) with cycle:", topologicalSortKahn(graphTSKahnJS_cycle));
// Expected: Graph contains a cycle! null
```

### TypeScript
```typescript
interface GraphTSKahnAdjListTS {
    [key: string]: string[];
}

function topologicalSortKahnTS(graph: GraphTSKahnAdjListTS): string[] | null {
    const inDegree: { [key: string]: number } = {};
    const queue: string[] = [];
    const result: string[] = [];

    // Initialize in-degrees
    for (let node in graph) {
        inDegree[node] = 0;
    }

    for (let node in graph) {
        for (let neighbor of graph[node]) {
            if (!(neighbor in inDegree)) {
                inDegree[neighbor] = 0; // Ensure all nodes are initialized
            }
            inDegree[neighbor]++;
        }
    }

    // Add all nodes with in-degree 0 to the queue
    for (let node in inDegree) {
        if (inDegree[node] === 0) {
            queue.push(node);
        }
    }

    while (queue.length > 0) {
        let u = queue.shift()!;
        result.push(u);

        for (let v of graph[u]) {
            inDegree[v]--;
            if (inDegree[v] === 0) {
                queue.push(v);
            }
        }
    }

    // Check for cycle
    if (result.length !== Object.keys(inDegree).length) {
        console.error("Graph contains a cycle!");
        return null;
    }

    return result;
}

const graphTSKahnTS: GraphTSKahnAdjListTS = {
    'A': ['C'],
    'B': ['C', 'D'],
    'C': ['E'],
    'D': ['F'],
    'E': ['F'],
    'F': []
};

console.log("Topological Sort (Kahn's TS):", topologicalSortKahnTS(graphTSKahnTS));
// Expected: [ 'A', 'B', 'C', 'D', 'E', 'F' ] (or [ 'B', 'A', 'C', 'D', 'E', 'F' ] etc.)

const graphTSKahnTS_cycle: GraphTSKahnAdjListTS = {
    'A': ['B'],
    'B': ['C'],
    'C': ['A']
};
console.log("Topological Sort (Kahn's TS) with cycle:", topologicalSortKahnTS(graphTSKahnTS_cycle));
// Expected: Graph contains a cycle! null
```

### Python
```python
from collections import deque

def topological_sort_kahn(graph):
    in_degree = {node: 0 for node in graph}
    for node in graph:
        for neighbor in graph[node]:
            if neighbor not in in_degree: # Handle nodes that are only targets of edges
                in_degree[neighbor] = 0
            in_degree[neighbor] += 1

    queue = deque([node for node, degree in in_degree.items() if degree == 0])
    result = []

    while queue:
        u = queue.popleft()
        result.append(u)

        for v in graph.get(u, []): # Use .get to handle nodes with no outgoing edges
            in_degree[v] -= 1
            if in_degree[v] == 0:
                queue.append(v)

    if len(result) != len(in_degree): # Check against all unique nodes found
        print("Graph contains a cycle!")
        return None

    return result

graph_tskahn_py = {
    'A': ['C'],
    'B': ['C', 'D'],
    'C': ['E'],
    'D': ['F'],
    'E': ['F'],
    'F': []
}

print("Topological Sort (Kahn's Python):", topological_sort_kahn(graph_tskahn_py))
# Expected: ['A', 'B', 'C', 'D', 'E', 'F'] (or ['B', 'A', 'C', 'D', 'E', 'F'] etc.)

graph_tskahn_py_cycle = {
    'A': ['B'],
    'B': ['C'],
    'C': ['A']
}
print("Topological Sort (Kahn's Python) with cycle:", topological_sort_kahn(graph_tskahn_py_cycle))
# Expected: Graph contains a cycle! None
```

### PHP
```php
<?php

function topologicalSortKahnPHP(array $graph): ?array {
    $inDegree = [];
    $queue = new SplQueue();
    $result = [];

    // Initialize in-degrees for all nodes (including those only as targets)
    foreach ($graph as $node => $neighbors) {
        if (!isset($inDegree[$node])) {
            $inDegree[$node] = 0;
        }
        foreach ($neighbors as $neighbor) {
            if (!isset($inDegree[$neighbor])) {
                $inDegree[$neighbor] = 0;
            }
            $inDegree[$neighbor]++;
        }
    }

    // Add all nodes with in-degree 0 to the queue
    foreach ($inDegree as $node => $degree) {
        if ($degree === 0) {
            $queue->enqueue($node);
        }
    }

    while (!$queue->isEmpty()) {
        $u = $queue->dequeue();
        $result[] = $u;

        if (isset($graph[$u])) { // Check if node has outgoing edges
            foreach ($graph[$u] as $v) {
                $inDegree[$v]--;
                if ($inDegree[$v] === 0) {
                    $queue->enqueue($v);
                }
            }
        }
    }

    // Check for cycle
    if (count($result) !== count($inDegree)) {
        echo "Graph contains a cycle!\n";
        return null;
    }

    return $result;
}

$graphTSKahnPHP = [
    'A' => ['C'],
    'B' => ['C', 'D'],
    'C' => ['E'],
    'D' => ['F'],
    'E'=> ['F'],
    'F' => []
];

echo "Topological Sort (Kahn's PHP): " . implode(", ", topologicalSortKahnPHP($graphTSKahnPHP)) . "\n";
// Expected: A, B, C, D, E, F (or B, A, C, D, E, F etc.)

$graphTSKahnPHP_cycle = [
    'A' => ['B'],
    'B' => ['C'],
    'C' => ['A']
];
echo "Topological Sort (Kahn's PHP) with cycle:\n";
print_r(topologicalSortKahnPHP($graphTSKahnPHP_cycle));
// Expected: Graph contains a cycle! null

?>
```
```