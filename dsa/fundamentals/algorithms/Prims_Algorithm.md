# Prim's Algorithm

## Description
Prim's algorithm is a greedy algorithm that finds a minimum spanning tree (MST) for a weighted undirected graph. This means it finds a subset of the edges that forms a tree that includes every vertex, where the total weight of all the edges in the tree is minimized. The algorithm operates by growing a tree from an arbitrary starting vertex, adding the cheapest available edge that connects a vertex in the tree to one outside the tree.

## When to Use
- When you need to find the minimum cost to connect all vertices in a weighted, undirected graph.
- In network design to minimize the cost of laying cables or connecting computers.
- For clustering algorithms.
- In circuit design to minimize wire length.

## Time Complexity
- **With Adjacency Matrix:** O(V^2)
- **With Adjacency List and Binary Heap (Priority Queue):** O(E log V) or O(E + V log V)

## Space Complexity
- O(V + E)

## Implementations (using Adjacency List and Priority Queue)

### JavaScript
```javascript
class PriorityQueuePrim {
    constructor() {
        this.values = [];
    }

    enqueue(val, priority) {
        this.values.push({ val, priority });
        this.sort();
    }

    dequeue() {
        return this.values.shift();
    }

    sort() {
        this.values.sort((a, b) => a.priority - b.priority);
    }
}

function primsAlgorithm(graph) {
    const mst = [];
    const visited = {};
    const pq = new PriorityQueuePrim();

    let startNode = Object.keys(graph)[0];
    visited[startNode] = true;

    // Add all edges from the startNode to the priority queue
    for (let neighbor in graph[startNode]) {
        pq.enqueue([startNode, neighbor], graph[startNode][neighbor]);
    }

    while (pq.values.length && mst.length < Object.keys(graph).length - 1) {
        let { val: [v1, v2], priority: weight } = pq.dequeue();

        if (visited[v1] && visited[v2]) {
            continue;
        }

        let newNode = visited[v1] ? v2 : v1;
        visited[newNode] = true;
        mst.push({ from: v1, to: v2, weight: weight });

        for (let neighbor in graph[newNode]) {
            if (!visited[neighbor]) {
                pq.enqueue([newNode, neighbor], graph[newNode][neighbor]);
            }
        }
    }

    return mst;
}

const graphPrimJS = {
    A: { B: 2, D: 3 },
    B: { A: 2, C: 1, D: 4 },
    C: { B: 1, E: 5 },
    D: { A: 3, B: 4, E: 6 },
    E: { C: 5, D: 6 }
};

console.log("Prim's Algorithm (JS):", primsAlgorithm(graphPrimJS));
/* Expected (order may vary):
[
  { from: 'A', to: 'B', weight: 2 },
  { from: 'B', to: 'C', weight: 1 },
  { from: 'A', to: 'D', weight: 3 },
  { from: 'C', to: 'E', weight: 5 }
]
*/
```

### TypeScript
```typescript
interface GraphPrimAdjListTS {
    [key: string]: { [neighbor: string]: number };
}

interface PriorityQueueNodePrimTS {
    val: [string, string]; // [from, to]
    priority: number; // weight
}

class PriorityQueuePrimTS {
    values: PriorityQueueNodePrimTS[] = [];

    enqueue(val: [string, string], priority: number): void {
        this.values.push({ val, priority });
        this.sort();
    }

    dequeue(): PriorityQueueNodePrimTS | undefined {
        return this.values.shift();
    }

    private sort(): void {
        this.values.sort((a, b) => a.priority - b.priority);
    }
}

function primsAlgorithmTS(graph: GraphPrimAdjListTS): { from: string, to: string, weight: number }[] {
    const mst: { from: string, to: string, weight: number }[] = [];
    const visited: { [key: string]: boolean } = {};
    const pq = new PriorityQueuePrimTS();

    let startNode = Object.keys(graph)[0];
    visited[startNode] = true;

    for (let neighbor in graph[startNode]) {
        pq.enqueue([startNode, neighbor], graph[startNode][neighbor]);
    }

    while (pq.values.length && mst.length < Object.keys(graph).length - 1) {
        let dequeued = pq.dequeue();
        if (!dequeued) continue;

        let { val: [v1, v2], priority: weight } = dequeued;

        if (visited[v1] && visited[v2]) {
            continue;
        }

        let newNode = visited[v1] ? v2 : v1;
        visited[newNode] = true;
        mst.push({ from: v1, to: v2, weight: weight });

        for (let neighbor in graph[newNode]) {
            if (!visited[neighbor]) {
                pq.enqueue([newNode, neighbor], graph[newNode][neighbor]);
            }
        }
    }

    return mst;
}

const graphPrimTS: GraphPrimAdjListTS = {
    A: { B: 2, D: 3 },
    B: { A: 2, C: 1, D: 4 },
    C: { B: 1, E: 5 },
    D: { A: 3, B: 4, E: 6 },
    E: { C: 5, D: 6 }
};

console.log("Prim's Algorithm (TS):", primsAlgorithmTS(graphPrimTS));
/* Expected (order may vary):
[
  { from: 'A', to: 'B', weight: 2 },
  { from: 'B', to: 'C', weight: 1 },
  { from: 'A', to: 'D', weight: 3 },
  { from: 'C', to: 'E', weight: 5 }
]
*/
```

### Python
```python
import heapq

def prims_algorithm(graph):
    mst = []
    visited = set()
    priority_queue = []

    start_node = list(graph.keys())[0]
    visited.add(start_node)

    for neighbor, weight in graph[start_node].items():
        heapq.heappush(priority_queue, (weight, start_node, neighbor))

    while priority_queue and len(mst) < len(graph) - 1:
        weight, v1, v2 = heapq.heappop(priority_queue)

        if v1 in visited and v2 in visited:
            continue

        new_node = v2 if v1 in visited else v1
        visited.add(new_node)
        mst.append({'from': v1, 'to': v2, 'weight': weight})

        for neighbor, edge_weight in graph[new_node].items():
            if neighbor not in visited:
                heapq.heappush(priority_queue, (edge_weight, new_node, neighbor))

    return mst

graph_prim_py = {
    'A': {'B': 2, 'D': 3},
    'B': {'A': 2, 'C': 1, 'D': 4},
    'C': {'B': 1, 'E': 5},
    'D': {'A': 3, 'B': 4, 'E': 6},
    'E': {'C': 5, 'D': 6}
}

print("Prim's Algorithm (Python):", prims_algorithm(graph_prim_py))
# Expected (order may vary):
# [{'from': 'B', 'to': 'C', 'weight': 1}, {'from': 'A', 'to': 'B', 'weight': 2}, {'from': 'A', 'to': 'D', 'weight': 3}, {'from': 'C', 'to': 'E', 'weight': 5}]
```

### PHP
```php
<?php

class PriorityQueuePrimPHP extends SplPriorityQueue {
    public function compare($priority1, $priority2): int {
        return $priority2 <=> $priority1; // Min-priority queue
    }
}

function primsAlgorithmPHP(array $graph): array {
    $mst = [];
    $visited = [];
    $pq = new PriorityQueuePrimPHP();

    $nodes = array_keys($graph);
    if (empty($nodes)) {
        return [];
    }
    $startNode = $nodes[0];
    $visited[$startNode] = true;

    foreach ($graph[$startNode] as $neighbor => $weight) {
        $pq->insert([$startNode, $neighbor], $weight);
    }

    while (!$pq->isEmpty() && count($mst) < count($graph) - 1) {
        $edge = $pq->extract();
        $weight = $pq->current(); // Get the priority (weight)
        $v1 = $edge[0];
        $v2 = $edge[1];

        if (isset($visited[$v1]) && isset($visited[$v2])) {
            continue;
        }

        $newNode = isset($visited[$v1]) ? $v2 : $v1;
        $visited[$newNode] = true;
        $mst[] = ['from' => $v1, 'to' => $v2, 'weight' => $weight];

        foreach ($graph[$newNode] as $neighbor => $edgeWeight) {
            if (!isset($visited[$neighbor])) {
                $pq->insert([$newNode, $neighbor], $edgeWeight);
            }
        }
    }

    return $mst;
}

$graphPrimPHP = [
    'A' => ['B' => 2, 'D' => 3],
    'B' => ['A' => 2, 'C' => 1, 'D' => 4],
    'C' => ['B' => 1, 'E' => 5],
    'D' => ['A' => 3, 'B' => 4, 'E' => 6],
    'E' => ['C' => 5, 'D' => 6]
];

echo "Prim's Algorithm (PHP):\n";
print_r(primsAlgorithmPHP($graphPrimPHP));
/* Expected (order may vary):
Array
(
    [0] => Array
        (
            [from] => B
            [to] => C
            [weight] => 1
        )

    [1] => Array
        (
            [from] => A
            [to] => B
            [weight] => 2
        )

    [2] => Array
        (
            [from] => A
            [to] => D
            [weight] => 3
        )

    [3] => Array
        (
            [from] => C
            [to] => E
            [weight] => 5
        )

)
*/

?>
```
```