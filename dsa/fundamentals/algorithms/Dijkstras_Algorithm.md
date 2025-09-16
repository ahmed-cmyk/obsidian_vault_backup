# Dijkstra's Algorithm

## Description
Dijkstra's algorithm is a shortest path algorithm for finding the shortest paths between nodes in a graph, which may represent, for example, road networks. It picks the unvisited vertex with the lowest distance, calculates the distance through it to each unvisited neighbor, and updates the neighbor's distance if smaller. It then marks the visited vertex as visited and repeats until all vertices are visited or the destination is reached.

## When to Use
- For finding the shortest path from a single source vertex to all other vertices in a graph with non-negative edge weights.
- In network routing protocols to find the optimal path for data packets.
- For GPS navigation systems to determine the shortest route between two locations.
- In various applications where minimizing cost or distance is crucial.

## Time Complexity
- **With Adjacency List and Binary Heap (Priority Queue):** O((V + E) log V)
- **With Adjacency List and Fibonacci Heap:** O(E + V log V)
- **With Adjacency Matrix:** O(V^2)

## Space Complexity
- O(V + E)

## Implementations (using Adjacency List and Priority Queue)

### JavaScript
```javascript
class PriorityQueueJS {
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

function dijkstra(graph, startNode) {
    const distances = {};
    const previous = {};
    const pq = new PriorityQueueJS();

    // Initialize distances and priority queue
    for (let vertex in graph) {
        if (vertex === startNode) {
            distances[vertex] = 0;
            pq.enqueue(vertex, 0);
        } else {
            distances[vertex] = Infinity;
            pq.enqueue(vertex, Infinity);
        }
        previous[vertex] = null;
    }

    while (pq.values.length) {
        let smallest = pq.dequeue().val;

        if (smallest || distances[smallest] !== Infinity) {
            for (let neighbor in graph[smallest]) {
                // Find neighbor node
                let nextNode = neighbor;
                // Calculate new distance to neighbor
                let candidate = distances[smallest] + graph[smallest][nextNode];
                let nextNeighbor = graph[smallest][nextNode];

                if (candidate < distances[nextNode]) {
                    distances[nextNode] = candidate;
                    previous[nextNode] = smallest;
                    pq.enqueue(nextNode, candidate);
                }
            }
        }
    }

    return distances;
}

const graphJS = {
    A: { B: 4, C: 2 },
    B: { A: 4, E: 3 },
    C: { A: 2, D: 2, F: 4 },
    D: { C: 2, E: 3, F: 1 },
    E: { B: 3, D: 3, F: 1 },
    F: { C: 4, D: 1, E: 1 }
};

console.log("Dijkstra's (JS) from A:", dijkstra(graphJS, "A"));
// Expected: { A: 0, B: 4, C: 2, D: 4, E: 5, F: 5 }
```

### TypeScript
```typescript
interface GraphAdjListTS {
    [key: string]: { [neighbor: string]: number };
}

interface PriorityQueueNodeTS<T> {
    val: T;
    priority: number;
}

class PriorityQueueTS<T> {
    values: PriorityQueueNodeTS<T>[] = [];

    enqueue(val: T, priority: number): void {
        this.values.push({ val, priority });
        this.sort();
    }

    dequeue(): PriorityQueueNodeTS<T> | undefined {
        return this.values.shift();
    }

    private sort(): void {
        this.values.sort((a, b) => a.priority - b.priority);
    }
}

function dijkstraTS(graph: GraphAdjListTS, startNode: string): { [node: string]: number } {
    const distances: { [node: string]: number } = {};
    const previous: { [node: string]: string | null } = {};
    const pq = new PriorityQueueTS<string>();

    for (let vertex in graph) {
        if (vertex === startNode) {
            distances[vertex] = 0;
            pq.enqueue(vertex, 0);
        } else {
            distances[vertex] = Infinity;
            pq.enqueue(vertex, Infinity);
        }
        previous[vertex] = null;
    }

    while (pq.values.length) {
        let smallest = pq.dequeue()?.val;

        if (smallest && distances[smallest] !== Infinity) {
            for (let neighbor in graph[smallest]) {
                let candidate = distances[smallest] + graph[smallest][neighbor];

                if (candidate < distances[neighbor]) {
                    distances[neighbor] = candidate;
                    previous[neighbor] = smallest;
                    pq.enqueue(neighbor, candidate);
                }
            }
        }
    }

    return distances;
}

const graphTS: GraphAdjListTS = {
    A: { B: 4, C: 2 },
    B: { A: 4, E: 3 },
    C: { A: 2, D: 2, F: 4 },
    D: { C: 2, E: 3, F: 1 },
    E: { B: 3, D: 3, F: 1 },
    F: { C: 4, D: 1, E: 1 }
};

console.log("Dijkstra's (TS) from A:", dijkstraTS(graphTS, "A"));
// Expected: { A: 0, B: 4, C: 2, D: 4, E: 5, F: 5 }
```

### Python
```python
import heapq

def dijkstra(graph, start_node):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start_node] = 0
    priority_queue = [(0, start_node)] # (distance, vertex)
    previous = {vertex: None for vertex in graph}

    while priority_queue:
        current_distance, current_vertex = heapq.heappop(priority_queue)

        if current_distance > distances[current_vertex]:
            continue

        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight

            if distance < distances[neighbor]:
                distances[neighbor] = distance
                previous[neighbor] = current_vertex
                heapq.heappush(priority_queue, (distance, neighbor))

    return distances

graph_py = {
    'A': {'B': 4, 'C': 2},
    'B': {'A': 4, 'E': 3},
    'C': {'A': 2, 'D': 2, 'F': 4},
    'D': {'C': 2, 'E': 3, 'F': 1},
    'E': {'B': 3, 'D': 3, 'F': 1},
    'F': {'C': 4, 'D': 1, 'E': 1}
}

print("Dijkstra's (Python) from A:", dijkstra(graph_py, 'A'))
// Expected: {'A': 0, 'B': 4, 'C': 2, 'D': 4, 'E': 5, 'F': 5}
```

### PHP
```php
<?php

class PriorityQueuePHP extends SplPriorityQueue {
    public function compare($priority1, $priority2): int {
        return $priority2 <=> $priority1; // Min-priority queue
    }
}

function dijkstraPHP(array $graph, string $startNode): array {
    $distances = [];
    $previous = [];
    $pq = new PriorityQueuePHP();

    foreach ($graph as $vertex => $edges) {
        $distances[$vertex] = INF;
        $previous[$vertex] = null;
    }
    $distances[$startNode] = 0;
    $pq->insert($startNode, 0);

    while (!$pq->isEmpty()) {
        $currentDistance = $pq->current();
        $currentVertex = $pq->extract();

        if ($currentDistance > $distances[$currentVertex]) {
            continue;
        }

        foreach ($graph[$currentVertex] as $neighbor => $weight) {
            $distance = $currentDistance + $weight;

            if ($distance < $distances[$neighbor]) {
                $distances[$neighbor] = $distance;
                $previous[$neighbor] = $currentVertex;
                $pq->insert($neighbor, $distance);
            }
        }
    }

    return $distances;
}

$graphPHP = [
    'A' => ['B' => 4, 'C' => 2],
    'B' => ['A' => 4, 'E' => 3],
    'C' => ['A' => 2, 'D' => 2, 'F' => 4],
    'D' => ['C' => 2, 'E' => 3, 'F' => 1],
    'E' => ['B' => 3, 'D' => 3, 'F' => 1],
    'F' => ['C' => 4, 'D' => 1, 'E' => 1]
];

echo "Dijkstra's (PHP) from A:\n";
print_r(dijkstraPHP($graphPHP, 'A'));
/* Expected:
Array
(
    [A] => 0
    [B] => 4
    [C] => 2
    [D] => 4
    [E] => 5
    [F] => 5
)
*/

?>
```
```