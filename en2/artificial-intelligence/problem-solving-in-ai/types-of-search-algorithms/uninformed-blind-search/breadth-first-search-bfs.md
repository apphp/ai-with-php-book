# Breadth-First Search (BFS)

Breadth-First Search is a widely used search strategy for traversing trees or graphs. It explores nodes level by level, expanding all successor nodes at the current depth before moving on to the next layer. This systematic breadthwise exploration is what gives BFS its name.

The BFS algorithm starts at the root node and explores all its immediate child nodes before advancing to their successors. It is an example of a general-graph search algorithm and is implemented using a **FIFO (First-In-First-Out)** queue data structure.

### **Key Characteristics**

1. **Level-by-Level Exploration**:\
   BFS explores the graph or tree level by level, starting from the root node and visiting all its neighbors before moving to the next level.
2. **Queue-Based Mechanism**:\
   BFS uses a queue data structure to manage the nodes to be visited. Nodes are added to the queue as they are discovered and removed in the order they were added, ensuring a First-In-First-Out (FIFO) traversal pattern.
3. **Shortest Path Guarantee**:\
   For unweighted graphs, BFS guarantees the shortest path (i.e., the minimum number of edges) from the source node to any other node.
4. **Visited List or Set**:\
   BFS relies on a mechanism, such as a _visited_ list or set, to track nodes that have already been explored. This avoids revisiting nodes and helps prevent infinite loops in cyclic graphs.
5. **Graph Traversal**:\
   BFS can be applied to both directed and undirected graphs, as well as weighted graphs where edge weights are uniform or ignored.
6. **Complete Search**:\
   BFS ensures that all reachable nodes are visited. This is especially useful for problems where every node or state needs to be explored.
7. **Parallel Exploration**:\
   Unlike DFS, which focuses on depth, BFS explores all possibilities at the current level before advancing, providing a breadth-oriented view of the graph or tree.

**Complexity**

* **Time Complexity**: $$O(b^d)$$, where  is the branching factor, and  is the depth of the shallowest goal.
* **Space Complexity**: $$O(b^d)$$.
* **Use Case**: Maze-solving or traversing social networks.

These characteristics make BFS an excellent choice for problems requiring exhaustive exploration or shortest-path calculations in unweighted graphs.

BFS is a fundamental algorithm known for its systematic and predictable nature, making it ideal for scenarios requiring exhaustive exploration or guaranteed minimal paths. However, its high resource requirements can pose challenges for large or deep search spaces.

### Advantages

1. **Guaranteed Solution**: BFS ensures a solution is found if one exists within the search space.
2. **Optimal Solution**: For problems with multiple solutions, BFS identifies the solution requiring the fewest steps.
3. **Shortest Path**: BFS is particularly effective for finding the shortest path to the goal, as it explores all nodes at the same depth before delving deeper.
4. **Easy to Understand**: BFS is straightforward to implement and interpret, making it easy to rank paths based on their hierarchical depth.

### Disadvantages

1. **High Memory Usage**: BFS can consume significant memory, as it must store all nodes at the current level to expand to the next level.
2. **Time-Consuming**: If the solution lies far from the root node, BFS may require considerable time to locate it.
3. **Inefficiency in Deep Trees**: BFS is inefficient for deeply nested spaces, as it thoroughly explores all nodes at each level before progressing to the next.

### Example

In the tree structure below, we illustrate the traversal process using the BFS algorithm, starting from the root node **S** and proceeding to the goal node **K** . The BFS algorithm explores the tree layer by layer, following the path indicated by the dotted arrows. \
\
The sequence of nodes visited during traversal will be:

$$S→A→B→C→D→G→H→E→F→I→K$$

<div align="left"><figure><img src="../../../../.gitbook/assets/image (35).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Complexity, **Completeness** and Optimality

#### **Time Complexity:**&#x20;

The time complexity of the BFS algorithm is determined by the number of nodes traversed until the shallowest solution node is found. Let:

* $$d$$ : The depth of the shallowest solution.
* $$b$$ : The branching factor, which is the maximum number of child nodes for any node.

At depth $$d$$ , the total number of nodes traversed is:  $$T (b) = 1+b^2+b^3+...+ b^d= O (b^d)$$

#### **Space Complexity:**&#x20;

The space complexity of BFS is determined by the memory required to store the frontier, which is the set of all nodes at the deepest level d . Since BFS explores level by level, the maximum number of nodes stored in memory corresponds to the size of the last level: $$O(b^d)$$.

#### **Completeness:**&#x20;

BFS is complete, meaning that it is guaranteed to find a solution if one exists, provided the goal node is at a finite depth in the search tree.

#### **Optimality:**&#x20;

BFS is optimal if the path cost is a non-decreasing function of the depth of the node. For example, in a uniform-cost search where all step costs are equal, BFS will find the optimal solution.

### Breadth-First Search with PHP

In PHP  it can be written as a class `UninformedSearchGraph` with implementation of a set of graph operations.

<details>

<summary>Example of Class UninformedSearchGraph</summary>

```php
class UninformedSearchGraph {
    private array $adjacencyList;
    private array $levels;
    // Store edge weights
    private array $weights;

    public function __construct() {
        $this->adjacencyList = [];
        $this->levels = [];
        $this->weights = [];
    }

    public function addVertex(string $vertex, int $level = -1): void {
        if (!isset($this->adjacencyList[$vertex])) {
            $this->adjacencyList[$vertex] = [];
            $this->levels[$vertex] = $level;
        }
    }

    public function addEdge(string $vertex1, string $vertex2, float $weight = 1.0): void {
        if (!isset($this->adjacencyList[$vertex1]) || !isset($this->adjacencyList[$vertex2])) {
            throw new InvalidArgumentException("Both vertices must exist in the graph.");
        }

        $this->adjacencyList[$vertex2][] = $vertex1;
        // For undirected graph
        $this->adjacencyList[$vertex1][] = $vertex2;

        // Store weights for both directions
        $this->weights["$vertex1->$vertex2"] = $weight;
        $this->weights["$vertex2->$vertex1"] = $weight;
    }

    public function bfs(string $startVertex): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph.");
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
            throw new InvalidArgumentException("Start vertex does not exist in the graph.");
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

    public function dls(string $startVertex, int $maxDepth, string $target = null): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph.");
        }

        $visited = [];
        $path = [];
        $found = false;

        // Helper function for recursive DLS
        $dlsRecursive = function(string $vertex, int $depth) use (&$dlsRecursive, &$visited, &$path, &$found, $maxDepth, $target): void {
            // Mark current vertex as visited
            $visited[$vertex] = true;

            // Add vertex to path
            $path[] = [
                'vertex' => $vertex,
                'level' => $this->levels[$vertex],
                'depth' => $depth
            ];

            // If we found the target, mark as found
            if ($vertex === $target) {
                $found = true;
                return;
            }

            // If we've reached max depth, stop exploring this path
            if ($depth >= $maxDepth) {
                return;
            }

            // Visit all adjacent vertices
            foreach ($this->adjacencyList[$vertex] as $neighbor) {
                if (!isset($visited[$neighbor]) && !$found) {
                    $dlsRecursive($neighbor, $depth + 1);
                }
            }

            // If this path didn't lead to the target and we're backtracking,
            // we can optionally remove this vertex from visited to allow it
            // to be visited again through a different path
            if (!$found) {
                unset($visited[$vertex]);
            }
        };

        // Start DLS from the given vertex at depth 0
        $dlsRecursive($startVertex, 0);

        return [
            'path' => $path,
            'found' => $found,
            'maxDepth' => $maxDepth
        ];
    }

    public function iddfs(string $startVertex, string $target = null, int $maxIterations = 100): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph.");
        }

        $allPaths = [];
        $depth = 0;

        // Iteratively increase depth until target is found or max depth is reached
        while ($depth < $maxIterations) {
            $result = $this->dls($startVertex, $depth, $target);
            $allPaths[] = [
                'depth_limit' => $depth,
                'path' => $result['path'],
                'found' => $result['found']
            ];

            // If target is found, return all paths explored
            if ($result['found']) {
                return [
                    'success' => true,
                    'final_depth' => $depth,
                    'paths' => $allPaths
                ];
            }

            $depth++;
        }

        // If target wasn't found within maxIterations
        return [
            'success' => false,
            'final_depth' => $depth - 1,
            'paths' => $allPaths
        ];
    }

    public function ucs(string $startVertex, string $targetVertex = null): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph");
        }

        $pq = new SplPriorityQueue();
        $pq->setExtractFlags(SplPriorityQueue::EXTR_BOTH);

        $costs = [$startVertex => 0];
        $visited = [];
        $previous = [$startVertex => null];  // Track the previous node
        $path = [];
        $explored = [];  // Track all explored nodes

        $pq->insert($startVertex, 0);

        while (!$pq->isEmpty()) {
            $current = $pq->extract();
            $currentVertex = $current['data'];
            $currentCost = -$current['priority'];

            if (isset($visited[$currentVertex])) {
                continue;
            }

            $visited[$currentVertex] = true;
            $explored[] = [
                'vertex' => $currentVertex,
                'level' => $this->levels[$currentVertex],
                'cost' => $currentCost
            ];

            if ($currentVertex === $targetVertex) {
                break;
            }

            foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
                $weight = $this->weights["$currentVertex->$neighbor"] ?? 1.0;
                $newCost = $costs[$currentVertex] + $weight;

                if (!isset($costs[$neighbor]) || $newCost < $costs[$neighbor]) {
                    $costs[$neighbor] = $newCost;
                    $previous[$neighbor] = $currentVertex;  // Store the previous node
                    $pq->insert($neighbor, -$newCost);
                }
            }
        }

        // Reconstruct the optimal path
        $optimalPath = [];
        $current = $targetVertex;
        while ($current !== null) {
            $optimalPath[] = [
                'vertex' => $current,
                'level' => $this->levels[$current],
                'cost' => $costs[$current]
            ];
            $current = $previous[$current];
        }

        return [
            'success' => isset($visited[$targetVertex]),
            'explored' => $explored,  // All nodes explored during search
            'optimalPath' => array_reverse($optimalPath),  // The actual optimal path
            'cost' => $costs[$targetVertex] ?? INF
        ];
    }

    public function bds(string $startVertex, string $targetVertex): array {
        if (!isset($this->adjacencyList[$startVertex]) || !isset($this->adjacencyList[$targetVertex])) {
            throw new InvalidArgumentException("Both start and target vertices must exist in the graph.");
        }

        // Initialize forward and backward search queues
        $forwardQueue = new SplQueue();
        $backwardQueue = new SplQueue();

        // Initialize visited sets and parent tracking for both directions
        $forwardVisited = [$startVertex => true];
        $backwardVisited = [$targetVertex => true];
        $forwardParent = [$startVertex => null];
        $backwardParent = [$targetVertex => null];

        // Initialize path tracking
        $forwardPath = [];
        $backwardPath = [];
        $intersectionVertex = null;

        // Add start and target vertices to their respective queues
        $forwardQueue->enqueue($startVertex);
        $backwardQueue->enqueue($targetVertex);

        while (!$forwardQueue->isEmpty() && !$backwardQueue->isEmpty()) {
            // Process forward search
            $intersectionVertex = $this->processBdsQueue(
                $forwardQueue,
                $forwardVisited,
                $backwardVisited,
                $forwardParent,
                $forwardPath,
                'forward'
            );

            if ($intersectionVertex !== null) {
                return $this->constructBdsPath(
                    $intersectionVertex,
                    $forwardParent,
                    $backwardParent,
                    $forwardPath,
                    $backwardPath
                );
            }

            // Process backward search
            $intersectionVertex = $this->processBdsQueue(
                $backwardQueue,
                $backwardVisited,
                $forwardVisited,
                $backwardParent,
                $backwardPath,
                'backward'
            );

            if ($intersectionVertex !== null) {
                return $this->constructBdsPath(
                    $intersectionVertex,
                    $forwardParent,
                    $backwardParent,
                    $forwardPath,
                    $backwardPath
                );
            }
        }

        // No path found
        return [
            'success' => false,
            'path' => [],
            'forwardExplored' => $forwardPath,
            'backwardExplored' => $backwardPath
        ];
    }

    private function processBdsQueue(
        SplQueue $queue,
        array &$currentVisited,
        array $oppositeVisited,
        array &$parentMap,
        array &$pathTracking,
        string $direction
    ): ?string {
        if ($queue->isEmpty()) {
            return null;
        }

        $currentVertex = $queue->dequeue();

        // Add to path tracking
        $pathTracking[] = [
            'vertex' => $currentVertex,
            'level' => $this->levels[$currentVertex],
            'direction' => $direction
        ];

        // Check neighbors
        foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
            // If we've found intersection with opposite search
            if (isset($oppositeVisited[$neighbor])) {
                return $neighbor;
            }

            // If not visited in current direction, add to queue
            if (!isset($currentVisited[$neighbor])) {
                $currentVisited[$neighbor] = true;
                $parentMap[$neighbor] = $currentVertex;
                $queue->enqueue($neighbor);
            }
        }

        return null;
    }

    private function constructBdsPath(
        string $intersectionVertex,
        array $forwardParent,
        array $backwardParent,
        array $forwardExplored,
        array $backwardExplored
    ): array {
        $path = [];

        // Construct path from start to intersection
        $current = $intersectionVertex;
        $forwardPath = [];
        while ($current !== null) {
            $forwardPath[] = [
                'vertex' => $current,
                'level' => $this->levels[$current]
            ];
            $current = $forwardParent[$current] ?? null;
        }
        $forwardPath = array_reverse($forwardPath);

        // Construct path from intersection to target
        $current = $backwardParent[$intersectionVertex] ?? null;
        $backwardPath = [];
        while ($current !== null) {
            $backwardPath[] = [
                'vertex' => $current,
                'level' => $this->levels[$current]
            ];
            $current = $backwardParent[$current] ?? null;
        }

        // Combine paths
        $path = array_merge($forwardPath, $backwardPath);

        return [
            'success' => true,
            'path' => $path,
            'forwardExplored' => $forwardExplored,
            'backwardExplored' => $backwardExplored,
            'intersectionVertex' => $intersectionVertex
        ];
    }

    // Add this helper method to print BDS results
    public function printBdsPath(array $result): void {
        if (!$result['success']) {
            echo "No path found between vertices!\n";
            return;
        }

        echo "\nNodes explored from start (forward direction):\n";
        foreach ($result['forwardExplored'] as $node) {
            echo sprintf("Node: %s (Level %d, Direction: %s)\n",
                $node['vertex'],
                $node['level'],
                $node['direction']
            );
        }

        echo "\nNodes explored from target (backward direction):\n";
        foreach ($result['backwardExplored'] as $node) {
            echo sprintf("Node: %s (Level %d, Direction: %s)\n",
                $node['vertex'],
                $node['level'],
                $node['direction']
            );
        }

        echo "\nFinal path found (intersection at {$result['intersectionVertex']}):\n";
        foreach ($result['path'] as $node) {
            echo sprintf("Node: %s (Level %d)\n",
                $node['vertex'],
                $node['level']
            );
        }
    }

    public function getAdjacencyList(): array {
        return $this->adjacencyList;
    }

    public function printPath(array $path): void {
        foreach ($path as $node) {
            echo sprintf("Node: %s (Level %d)\n", $node['vertex'], $node['level']);
        }
    }

    public function printUcsPath(array $result): void {
        if (!$result['success']) {
            echo "Target not found!\n";
            return;
        }

        echo "\nNodes explored during UCS (in order of exploration):\n";
        foreach ($result['explored'] as $node) {
            echo sprintf("Node: %s (Level %d, Cost %.2f)\n",
                $node['vertex'],
                $node['level'],
                $node['cost']
            );
        }

        echo "\nOptimal path found:\n";
        foreach ($result['optimalPath'] as $node) {
            echo sprintf("Node: %s (Level %d, Cost %.2f)\n",
                $node['vertex'],
                $node['level'],
                $node['cost']
            );
        }
        echo sprintf("Total Cost: %.2f\n", $result['cost']);
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
$graph = new UninformedSearchGraph();

// Add vertices
$graph->addVertex('S', 0);  // Level 0
$graph->addVertex('A', 1);  // Level 1
$graph->addVertex('B', 1);  // Level 1
$graph->addVertex('C', 2);  // Level 2
$graph->addVertex('D', 2);  // Level 2
$graph->addVertex('G', 2);  // Level 2
$graph->addVertex('H', 2);  // Level 2
$graph->addVertex('E', 3);  // Level 3
$graph->addVertex('F', 3);  // Level 3
$graph->addVertex('I', 3);  // Level 3
$graph->addVertex('K', 4);  // Level 4

// Add edges
$graph->addEdge('S', 'A');
$graph->addEdge('S', 'B');
$graph->addEdge('A', 'C');
$graph->addEdge('A', 'D');
$graph->addEdge('C', 'E');
$graph->addEdge('C', 'F');
$graph->addEdge('E', 'K');
$graph->addEdge('B', 'G');
$graph->addEdge('B', 'H');
$graph->addEdge('G', 'I');

// Perform DFS starting from 'S' to find 'K'
$bfsResult = $graph->bfs('S');

// Output the BFS traversal
echo "BFS traversal starting from vertex 'S':\n";
$graph->printPath($bfsResult);
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
