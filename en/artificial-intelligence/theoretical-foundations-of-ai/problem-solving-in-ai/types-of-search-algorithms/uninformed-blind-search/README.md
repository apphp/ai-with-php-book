# Uninformed (Blind) Search

Uninformed search algorithms operate without any domain-specific knowledge about the problem. These algorithms do not use information such as the location of the goal or the proximity of states to the goal. Instead, they traverse the search tree systematically, evaluating nodes based solely on their position within the tree. This lack of guidance often results in brute-force exploration of the search space, examining every possible state until the goal is found.

### Key Characteristics of Uninformed Search

1. **No Heuristics**: Relies purely on the structure of the search tree.
2. **Exhaustive Exploration**: Explores all possible nodes, making it complete but often inefficient.
3. **General Applicability**: Can solve any problem where the goal state can be identified.

### Types of Uninformed Search Algorithms

1. #### Breadth-First Search (BFS)
2. #### Depth-First Search (DFS)
3. #### Uniform Cost Search (UCS)
4. #### Iterative Deepening Depth-First Search (IDDFS)
5. #### Bidirectional Search (BDS)
6. **Depth-limited Search (DLS)**
7. **Random Walk Search (RWS)**

Let's take a closer look at all these types.

### Uninformed Search Algorithms with PHP

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

    public function rws(string $startVertex, string $targetVertex = null, int $maxSteps = 1000): array {
        if (!isset($this->adjacencyList[$startVertex])) {
            throw new InvalidArgumentException("Start vertex does not exist in the graph.");
        }

        $path = [];
        $visited = [];
        $currentVertex = $startVertex;
        $steps = 0;
        $found = false;

        // Add start vertex to path
        $path[] = [
            'vertex' => $currentVertex,
            'level' => $this->levels[$currentVertex],
            'step' => $steps
        ];

        while ($steps < $maxSteps) {
            // Check if we've found the target
            if ($targetVertex !== null && $currentVertex === $targetVertex) {
                $found = true;
                break;
            }

            // Get neighbors of current vertex
            $neighbors = $this->adjacencyList[$currentVertex];

            // If no neighbors, break
            if (empty($neighbors)) {
                break;
            }

            // Randomly select next vertex
            $nextVertex = $neighbors[array_rand($neighbors)];
            $steps++;

            // Track visited nodes (optional, can be removed if you want pure random walk)
            $visited[$currentVertex] = ($visited[$currentVertex] ?? 0) + 1;

            // Add to path
            $path[] = [
                'vertex' => $nextVertex,
                'level' => $this->levels[$nextVertex],
                'step' => $steps,
                'visits' => $visited[$nextVertex] ?? 0
            ];

            $currentVertex = $nextVertex;
        }

        return [
            'success' => $found,
            'path' => $path,
            'steps' => $steps,
            'maxSteps' => $maxSteps,
            'visited' => $visited
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

    // Add a helper method to print random search results
    public function printRwsPath(array $result): void {
        echo sprintf("\nRandom Search %s\n",
            $result['success'] ? "found target!" : "did not find target."
        );

        echo sprintf("Total steps taken: %d/%d\n",
            $result['steps'],
            $result['maxSteps']
        );

        echo "\nPath taken:\n";
        foreach ($result['path'] as $node) {
            echo sprintf("Step %d: Node %s (Level %d, Visits: %d)\n",
                $node['step'],
                $node['vertex'],
                $node['level'],
                $node['visits'] ?? 0
            );
        }

        echo "\nVisit counts:\n";
        foreach ($result['visited'] as $vertex => $count) {
            echo sprintf("Node %s: visited %d times\n", $vertex, $count);
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

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
