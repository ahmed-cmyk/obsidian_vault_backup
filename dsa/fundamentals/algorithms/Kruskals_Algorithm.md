# Kruskal's Algorithm

## Description
Kruskal's algorithm is a greedy algorithm that finds a minimum spanning tree (MST) for a connected, weighted undirected graph. It works by sorting all the edges in the graph in non-decreasing order of their weights. Then, it iterates through the sorted edges, adding an edge to the MST if it does not form a cycle with the already added edges. This process continues until V-1 edges (where V is the number of vertices) have been added to the MST.

## When to Use
- When you need to find the minimum cost to connect all vertices in a weighted, undirected graph, similar to Prim's algorithm.
- Often preferred over Prim's when the graph is sparse (has few edges).
- In network design, for example, to lay out electrical wiring, water pipes, or computer networks with minimum cost.

## Time Complexity
- O(E log E) or O(E log V) (dominated by sorting edges, where E is the number of edges and V is the number of vertices)

## Space Complexity
- O(V + E)

## Implementations (using Disjoint Set Union - DSU)

### JavaScript
```javascript
class DisjointSet {
    constructor(elements) {
        this.parent = {};
        this.rank = {};
        for (let el of elements) {
            this.parent[el] = el;
            this.rank[el] = 0;
        }
    }

    find(i) {
        if (this.parent[i] === i) {
            return i;
        }
        this.parent[i] = this.find(this.parent[i]);
        return this.parent[i];
    }

    union(i, j) {
        let rootI = this.find(i);
        let rootJ = this.find(j);

        if (rootI !== rootJ) {
            if (this.rank[rootI] < this.rank[rootJ]) {
                this.parent[rootI] = rootJ;
            } else if (this.rank[rootI] > this.rank[rootJ]) {
                this.parent[rootJ] = rootI;
            } else {
                this.parent[rootJ] = rootI;
                this.rank[rootI]++;
            }
            return true;
        }
        return false;
    }
}

function kruskalsAlgorithm(vertices, edges) {
    const mst = [];
    edges.sort((a, b) => a.weight - b.weight);

    const dsu = new DisjointSet(vertices);

    for (let i = 0; i < edges.length && mst.length < vertices.length - 1; i++) {
        const { u, v, weight } = edges[i];
        if (dsu.union(u, v)) {
            mst.push({ u, v, weight });
        }
    }

    return mst;
}

const verticesJS = ['A', 'B', 'C', 'D', 'E', 'F'];
const edgesJS = [
    { u: 'A', v: 'B', weight: 7 },
    { u: 'A', v: 'D', weight: 5 },
    { u: 'B', v: 'C', weight: 8 },
    { u: 'B', v: 'D', weight: 9 },
    { u: 'B', v: 'E', weight: 7 },
    { u: 'C', v: 'E', weight: 5 },
    { u: 'D', v: 'E', weight: 15 },
    { u: 'D', v: 'F', weight: 6 },
    { u: 'E', v: 'F', weight: 8 },
    { u: 'E', v: 'G', weight: 9 },
    { u: 'F', v: 'G', weight: 11 }
];

console.log("Kruskal's Algorithm (JS):", kruskalsAlgorithm(verticesJS, edgesJS));
/* Expected (order may vary):
[
  { u: 'A', v: 'D', weight: 5 },
  { u: 'C', v: 'E', weight: 5 },
  { u: 'D', v: 'F', weight: 6 },
  { u: 'A', v: 'B', weight: 7 },
  { u: 'E', v: 'G', weight: 9 }
]
*/
```

### TypeScript
```typescript
class DisjointSetTS<T> {
    private parent: { [key: string]: T };
    private rank: { [key: string]: number };

    constructor(elements: T[]) {
        this.parent = {};
        this.rank = {};
        for (let el of elements) {
            this.parent[String(el)] = el;
            this.rank[String(el)] = 0;
        }
    }

    find(i: T): T {
        if (this.parent[String(i)] === i) {
            return i;
        }
        this.parent[String(i)] = this.find(this.parent[String(i)]);
        return this.parent[String(i)];
    }

    union(i: T, j: T): boolean {
        let rootI = this.find(i);
        let rootJ = this.find(j);

        if (String(rootI) !== String(rootJ)) {
            if (this.rank[String(rootI)] < this.rank[String(rootJ)]) {
                this.parent[String(rootI)] = rootJ;
            } else if (this.rank[String(rootI)] > this.rank[String(rootJ)]) {
                this.parent[String(rootJ)] = rootI;
            } else {
                this.parent[String(rootJ)] = rootI;
                this.rank[String(rootI)]++;
            }
            return true;
        }
        return false;
    }
}

interface EdgeTS {
    u: string;
    v: string;
    weight: number;
}

function kruskalsAlgorithmTS(vertices: string[], edges: EdgeTS[]): EdgeTS[] {
    const mst: EdgeTS[] = [];
    edges.sort((a, b) => a.weight - b.weight);

    const dsu = new DisjointSetTS(vertices);

    for (let i = 0; i < edges.length && mst.length < vertices.length - 1; i++) {
        const { u, v, weight } = edges[i];
        if (dsu.union(u, v)) {
            mst.push({ u, v, weight });
        }
    }

    return mst;
}

const verticesTS: string[] = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
const edgesTS: EdgeTS[] = [
    { u: 'A', v: 'B', weight: 7 },
    { u: 'A', v: 'D', weight: 5 },
    { u: 'B', v: 'C', weight: 8 },
    { u: 'B', v: 'D', weight: 9 },
    { u: 'B', v: 'E', weight: 7 },
    { u: 'C', v: 'E', weight: 5 },
    { u: 'D', v: 'E', weight: 15 },
    { u: 'D', v: 'F', weight: 6 },
    { u: 'E', v: 'F', weight: 8 },
    { u: 'E', v: 'G', weight: 9 },
    { u: 'F', v: 'G', weight: 11 }
];

console.log("Kruskal's Algorithm (TS):", kruskalsAlgorithmTS(verticesTS, edgesTS));
/* Expected (order may vary):
[
  { u: 'A', v: 'D', weight: 5 },
  { u: 'C', v: 'E', weight: 5 },
  { u: 'D', v: 'F', weight: 6 },
  { u: 'A', v: 'B', weight: 7 },
  { u: 'E', v: 'G', weight: 9 }
]
*/
```

### Python
```python
class DisjointSet:
    def __init__(self, elements):
        self.parent = {el: el for el in elements}
        self.rank = {el: 0 for el in elements}

    def find(self, i):
        if self.parent[i] == i:
            return i
        self.parent[i] = self.find(self.parent[i])
        return self.parent[i]

    def union(self, i, j):
        root_i = self.find(i)
        root_j = self.find(j);

        if root_i != root_j:
            if self.rank[root_i] < self.rank[root_j]:
                self.parent[root_i] = root_j
            elif self.rank[root_i] > self.rank[root_j]:
                self.parent[root_j] = root_i
            else:
                self.parent[root_j] = root_i
                self.rank[root_i] += 1
            return True
        return False

def kruskals_algorithm(vertices, edges):
    mst = []
    edges.sort(key=lambda x: x['weight'])

    dsu = DisjointSet(vertices)

    for edge in edges:
        u, v, weight = edge['u'], edge['v'], edge['weight']
        if dsu.union(u, v):
            mst.append({'u': u, 'v': v, 'weight': weight})
        if len(mst) == len(vertices) - 1:
            break

    return mst

vertices_py = ['A', 'B', 'C', 'D', 'E', 'F', 'G']
edges_py = [
    {'u': 'A', 'v': 'B', 'weight': 7},
    {'u': 'A', 'v': 'D', 'weight': 5},
    {'u': 'B', 'v': 'C', 'weight': 8},
    {'u': 'B', 'v': 'D', 'weight': 9},
    {'u': 'B', 'v': 'E', 'weight': 7},
    {'u': 'C', 'v': 'E', 'weight': 5},
    {'u': 'D', 'v': 'E', 'weight': 15},
    {'u': 'D', 'v': 'F', 'weight': 6},
    {'u': 'E', 'v': 'F', 'weight': 8},
    {'u': 'E', 'v': 'G', 'weight': 9},
    {'u': 'F', 'v': 'G', 'weight': 11}
]

print("Kruskal's Algorithm (Python):", kruskals_algorithm(vertices_py, edges_py))
# Expected (order may vary):
# [{'u': 'A', 'v': 'D', 'weight': 5}, {'u': 'C', 'v': 'E', 'weight': 5}, {'u': 'D', 'v': 'F', 'weight': 6}, {'u': 'A', 'v': 'B', 'weight': 7}, {'u': 'B', 'v': 'E', 'weight': 7}, {'u': 'E', 'v': 'G', 'weight': 9}]
```

### PHP
```php
<?php

class DisjointSetPHP {
    private $parent = [];
    private $rank = [];

    public function __construct(array $elements) {
        foreach ($elements as $el) {
            $this->parent[$el] = $el;
            $this->rank[$el] = 0;
        }
    }

    public function find(string $i): string {
        if ($this->parent[$i] === $i) {
            return $i;
        }
        $this->parent[$i] = $this->find($this->parent[$i]);
        return $this->parent[$i];
    }

    public function union(string $i, string $j): bool {
        $rootI = $this->find($i);
        $rootJ = $this->find($j);

        if ($rootI !== $rootJ) {
            if ($this->rank[$rootI] < $this->rank[$rootJ]) {
                $this->parent[$rootI] = $rootJ;
            } else if ($this->rank[$rootI] > $this->rank[$rootJ]) {
                $this->parent[$rootJ] = $rootI;
            } else {
                $this->parent[$rootJ] = $rootI;
                $this->rank[$rootI]++;
            }
            return true;
        }
        return false;
    }
}

function kruskalsAlgorithmPHP(array $vertices, array $edges): array {
    $mst = [];
    usort($edges, function($a, $b) { return $a['weight'] <=> $b['weight']; });

    $dsu = new DisjointSetPHP($vertices);

    foreach ($edges as $edge) {
        if (count($mst) === count($vertices) - 1) {
            break;
        }
        $u = $edge['u'];
        $v = $edge['v'];
        $weight = $edge['weight'];

        if ($dsu->union($u, $v)) {
            $mst[] = ['u' => $u, 'v' => $v, 'weight' => $weight];
        }
    }

    return $mst;
}

$verticesPHP = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
$edgesPHP = [
    ['u' => 'A', 'v' => 'B', 'weight' => 7],
    ['u' => 'A', 'v' => 'D', 'weight' => 5],
    ['u' => 'B', 'v' => 'C', 'weight' => 8],
    ['u' => 'B', 'v' => 'D', 'weight' => 9],
    ['u' => 'B', 'v': 'E', 'weight' => 7],
    ['u' => 'C', 'v': 'E', 'weight' => 5],
    ['u' => 'D', 'v': 'E', 'weight' => 15],
    ['u' => 'D', 'v': 'F', 'weight' => 6],
    ['u' => 'E', 'v': 'F', 'weight' => 8],
    ['u': 'E', 'v': 'G', 'weight' => 9],
    ['u': 'F', 'v': 'G', 'weight' => 11]
];

echo "Kruskal's Algorithm (PHP):\n";
print_r(kruskalsAlgorithmPHP($verticesPHP, $edgesPHP));
/* Expected (order may vary):
Array
(
    [0] => Array
        (
            [u] => A
            [v] => D
            [weight] => 5
        )

    [1] => Array
        (
            [u] => C
            [v] => E
            [weight] => 5
        )

    [2] => Array
        (
            [u] => D
            [v] => F
            [weight] => 6
        )

    [3] => Array
        (
            [u] => A
            [v] => B
            [weight] => 7
        )

    [4] => Array
        (
            [u] => B
            [v] => E
            [weight] => 7
        )

    [5] => Array
        (
            [u] => E
            [v] => G
            [weight] => 9
        )

)
*/

?>
```
```