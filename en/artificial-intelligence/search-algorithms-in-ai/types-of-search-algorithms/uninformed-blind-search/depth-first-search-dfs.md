# Depth-First Search (DFS)

Depth-First Search (DFS) is a classic algorithm for traversing or searching through tree and graph data structures. As the name suggests, DFS explores as far down a branch as possible before backtracking to examine other paths. This behavior makes DFS particularly useful in scenarios where exploring deep hierarchies or paths is necessary. It relies on a **stack data structure** — either explicitly (using a manual stack) or implicitly (via recursion) — to manage the nodes being visited.

DFS starts at a root node, explores one path completely until it reaches a dead end or goal, and then backtracks to explore other unvisited paths. This recursive approach is not only efficient in certain cases but also forms the foundation for various computational techniques such as backtracking, a method for solving problems by exploring all possible solutions.

### Key Characteristics

1. **Depth-Oriented Exploration**:\
   DFS prioritizes depth over breadth. It dives deep into one branch of the graph or tree before moving to the next.
2. **Stack-Based Mechanism**:\
   The algorithm uses a stack to track the nodes along the current path. Recursion often simplifies this process by leveraging the call stack.
3. **Graph Traversal**:\
   DFS can be applied to both directed and undirected graphs. To prevent visiting the same node multiple times, a mechanism like a _visited_ list or set is often employed.
4. **Backtracking**:\
   DFS inherently supports backtracking, making it useful for problems like solving mazes, puzzles, or finding paths.

**Complexity**

* **Time Complexity**: $$O(b^m)$$, where is the maximum depth of the tree.
* **Space Complexity**: $$O(b \times m)$$.
* **Use Case**: Puzzle-solving where solutions are deep in the tree.

### Advantages

1. **Memory Efficient**:\
   Unlike Breadth-First Search (BFS), which requires memory proportional to the breadth of the graph, DFS only needs to store nodes in the current path, making it suitable for memory-constrained environments.
2. **Quick to Reach Goals**:\
   DFS can quickly reach the goal if the target is located deep within the search space, provided it traverses the correct path.
3. **Path Tracking**:\
   DFS naturally keeps track of the current route, making it easier to reconstruct paths or solutions once a goal is found.
4. **Supports Backtracking**:\
   By design, DFS supports the systematic exploration of all possibilities using backtracking, which is beneficial in constraint satisfaction problems.

### Disadvantages

1. **Revisiting Nodes**:\
   Without proper checks, DFS may revisit the same nodes repeatedly, leading to inefficiency and potentially incorrect results in cyclic graphs.
2. **Infinite Loops**:\
   In cases of infinite or very large graphs, DFS can fall into an infinite loop unless safeguards are in place to limit depth or track visited nodes.
3. **No Optimal Solutions**:\
   DFS does not guarantee the shortest path to a solution. For example, it may find a solution in a deeper branch before discovering a shorter path in a different branch.
4. **Blind Exploration**:\
   DFS explores blindly until it hits a dead end, which can result in a long runtime if the target is located in an area far removed from the initial exploration path.

Depth-First Search is a versatile and efficient algorithm for deep exploration of data structures. While it has its limitations, such as the risk of infinite loops and lack of optimality, its low memory usage and suitability for solving complex problems like puzzles and mazes make it an indispensable tool in the field of computer science.

### Example

In the search tree below, the flow of the depth-first search follows this order:&#x20;

$$Root node → Left node → Right node.$$

The traversal begins at the root node **S**, progressing to **A**, then **B**, followed by **D** and **E**. Since **E** has no successors and the goal node hasn’t been found, the algorithm backtracks to explore other branches. After backtracking, it moves to node **C** and finally to **G**, where it terminates upon finding the goal node.

<div align="left"><figure><img src="../../../../.gitbook/assets/image (145).png" alt="" width="563"><figcaption></figcaption></figure></div>

**Completeness**:&#x20;

The DFS algorithm is complete when operating within a finite state space, as it will explore every node in a finite search tree.

**Time Complexity**:&#x20;

The time complexity of DFS corresponds to the number of nodes traversed during the search. It is represented as:

$$T(n) = 1 + n_2 + n_3 + \dots + n_m = O(b^m)$$

where $$m$$ is the maximum depth of any node, which can be significantly larger than $$d$$, the depth of the shallowest solution.

**Space Complexity**:&#x20;

DFS requires memory to store only the current path from the root node, making its space complexity proportional to the size of the fringe, which is $$O(bm)$$.

**Optimality**:&#x20;

DFS is non-optimal because it may take many steps or incur high costs to reach the goal node, potentially bypassing shorter or less costly solutions.

### Breadth-First Search with PHP

In PHP  it can be written as a class Graph with implementation of a set of graph operations.

<details>

<summary>Example of Class Graph</summary>

```php
declare(strict_types=1);

class Graph {
    private array $adjacencyList;
    private array $levels;

    public function __construct() {
        $this->adjacencyList = [];
        $this->levels = [];
    }

    public function addVertex(string $vertex, int $level = -1): void {
        if (!isset($this->adjacencyList[$vertex])) {
            $this->adjacencyList[$vertex] = [];
            $this->levels[$vertex] = $level;
        }
    }

    public function addEdge(string $vertex1, string $vertex2): void {
        if (!isset($this->adjacencyList[$vertex1]) || !isset($this->adjacencyList[$vertex2])) {
            throw new InvalidArgumentException("Both vertices must exist in the graph");
        }

        $this->adjacencyList[$vertex1][] = $vertex2;
        $this->adjacencyList[$vertex2][] = $vertex1; // For undirected graph
    }

    public function bfs(string $startVertex): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph");
        }

        $visited = [];
        $queue = new SplQueue();
        $path = [];

        // Mark the starting vertex as visited and enqueue it
        $visited[$startVertex] = true;
        $queue->enqueue($startVertex);

        while (!$queue->isEmpty()) {
            $currentVertex = $queue->dequeue();

            // Add vertex to path
            $path[] = [
                'vertex' => $currentVertex,
                'level' => $this->levels[$currentVertex]
            ];

            // Get all adjacent vertices of the dequeued vertex
            foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
                if (!isset($visited[$neighbor])) {
                    $visited[$neighbor] = true;
                    $queue->enqueue($neighbor);
                }
            }
        }

        return $path;
    }

    public function dfs(string $startVertex, string $target = null): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph");
        }

        $visited = [];
        $path = [];

        // Helper function for recursive DFS
        $dfsRecursive = function(string $vertex) use (&$dfsRecursive, &$visited, &$path, $target): bool {
            // Mark current vertex as visited
            $visited[$vertex] = true;

            // Add vertex to path
            $path[] = [
                'vertex' => $vertex,
                'level' => $this->levels[$vertex]
            ];

            // If we found the target, stop the search
            if ($vertex === $target) {
                return true; // Target found
            }

            // Visit all adjacent vertices
            foreach ($this->adjacencyList[$vertex] as $neighbor) {
                if (!isset($visited[$neighbor])) {
                    if ($dfsRecursive($neighbor)) {
                        return true; // Target found in this path
                    }
                }
            }

            return false; // Target not found in this path
        };

        // Start DFS from the given vertex
        $dfsRecursive($startVertex);
        return $path;
    }

    public function getAdjacencyList(): array {
        return $this->adjacencyList;
    }

    public function printPath(array $path): void {
        foreach ($path as $node) {
            echo sprintf("Node: %s (Level %d)\n", $node['vertex'], $node['level']);
        }
    }

    // Helper method to print the adjacency list (for debugging)
    public function printGraph(): void {
        foreach ($this->adjacencyList as $vertex => $neighbors) {
            echo sprintf("%s (Level %d) -> %s\n",
                $vertex,
                $this->levels[$vertex],
                implode(', ', $neighbors)
            );
        }
    }
}

```

</details>

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new Graph();

// Add vertices with their levels
$graph->addVertex('S', 0);  // Level 0
$graph->addVertex('A', 1);  // Level 1
$graph->addVertex('H', 1);  // Level 1
$graph->addVertex('B', 2);  // Level 2
$graph->addVertex('C', 2);  // Level 2
$graph->addVertex('I', 2);  // Level 2
$graph->addVertex('J', 2);  // Level 2
$graph->addVertex('D', 3);  // Level 3
$graph->addVertex('E', 3);  // Level 3
$graph->addVertex('K', 3);  // Level 3 (First K)

// Add edges according to the tree structure
$graph->addEdge('S', 'A');
$graph->addEdge('S', 'H');
$graph->addEdge('A', 'B');
$graph->addEdge('A', 'C');
$graph->addEdge('B', 'D');
$graph->addEdge('B', 'E');
$graph->addEdge('C', 'K');
$graph->addEdge('H', 'I');
$graph->addEdge('H', 'J');

// Perform DFS starting from 'S' to find 'K'
$dfsResult = $graph->dfs('S', 'K');

// Output the DFS traversal
echo "DFS traversal starting from vertex 'S':\n";
$graph->printPath($dfsResult);
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
