# Greedy Search

Greedy search is an informed search algorithm that aims to expand the node closest to the goal, as estimated by a heuristic function . It takes a direct and straightforward approach, always choosing the path that seems most promising based on the heuristic value. The method is inspired by human intuition — choosing the option that appears best at each step without considering the overall problem structure. While simple and often efficient, greedy search is not guaranteed to find the optimal solution.

### Key Characteristics

1. **Heuristic-Driven Approach**:\
   Greedy search relies heavily on a heuristic function, , which estimates the cost or distance from a given node to the goal node. The algorithm prioritizes nodes with lower heuristic values, as they are considered “closer” to the goal.\
   Heuristic Function: $$h(x) =$$ Estimate of distance of node $$x$$ from the goal node.\
   Lower the value of $$h(x)$$, closer is the node from the goal.
2. **Node Expansion Strategy**:\
   Nodes are expanded based on their heuristic value. The node with the lowest is expanded first, reflecting the algorithm’s pursuit of the shortest perceived path to the goal.
3. **No Backtracking**:\
   Greedy search does not account for the total path cost from the start node to the current node; it only considers the heuristic to the goal. This makes it efficient but prone to getting stuck in local optima or dead ends.
4. **Algorithm Behavior**:\
   •  Starts at the initial node.\
   •  Evaluates and selects the node with the smallest from the frontier (open list).\
   • Repeats the process until the goal node is reached or no further nodes are available.

### Advantages

1. **Efficiency in Simple Problems**:\
   Greedy search can quickly find a solution for problems where the heuristic accurately reflects the path to the goal. Its simplicity and speed make it ideal for problems with well-behaved heuristics.
2. **Ease of Implementation**:\
   The algorithm is straightforward to code and understand, making it a popular choice for certain types of searches and optimization problems.
3. **Focus on Goal**:\
   By directly expanding nodes closest to the goal, greedy search minimizes unnecessary exploration, focusing on the most promising routes.

### Disadvantages

1. **Lack of Optimality:**\
   Greedy search does not guarantee the optimal solution. It may settle for a suboptimal solution if the heuristic misleads it toward a local minimum or a longer path.
2. **Incomplete Search:**\
   If the heuristic function is poorly designed or the search space is too complex, the algorithm might fail to find a solution even if one exists.
3. **Sensitivity to Heuristic:**\
   The success of greedy search is highly dependent on the quality of the heuristic. An inaccurate heuristic can lead to inefficient or incorrect results.
4. **No Cost Awareness:**\
   Greedy search does not account for the actual cost incurred from the start node to the current node, which can result in selecting paths that seem promising initially but are more expensive overall.

In conclusion, greedy search is a powerful algorithm when used in the right contexts. Its reliance on heuristic-driven expansion allows for rapid solutions but can also limit its effectiveness in scenarios with complex or deceptive search spaces. Understanding its strengths and limitations is key to applying it effectively in artificial intelligence and other computational problems.

### **Example**

Find the path from S to G using greedy search. The heuristic values $$h$$ of each node below the name of the node.

Starting from node S , we can move to either A (h = 9) or D (h = 5). Since D has the lower heuristic value, we select it. From D , we can proceed to either B (h = 4) or E (h = 3). Again, we choose E because it has the smaller heuristic value. Finally, from E , we move to G (h = 0), which is the goal node. The complete traversal is illustrated in the search tree below, highlighted in dashed red.

The sequence of nodes visited during traversal will be:&#x20;

$$S→D→E→G$$

<div align="left"><figure><img src="../../../../.gitbook/assets/image (37).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Complexity, **Completeness** and Optimality

#### Time Complexity:

The time complexity depends on the branching factor $$b$$ and the maximum depth $$m$$ of the search tree. In the worst case, the complexity is , as it may need to explore all nodes.

#### Space Complexity:

The space complexity is also $$O(b^m)$$, as it stores all generated nodes in memory, including the frontier.

#### Completeness:

Greedy search is incomplete. It may fail to find a solution if the search space is infinite or if the heuristic leads the search down a non-terminating path.

#### Optimality:

Greedy search is not optimal, as it does not consider the total cost of the path and may return a suboptimal solution.

In conclusion, greedy search is a powerful algorithm when used in the right contexts. Its reliance on heuristic-driven expansion allows for rapid solutions but can also limit its effectiveness in scenarios with complex or deceptive search spaces.

### Greedy Search with PHP

In PHP  it can be written as a class `InformedSearchGraph` with implementation of a set of graph operations.

<details>

<summary>Example of Class InformedSearchGraph</summary>

```php
class InformedSearchGraph {
    private array $adjacencyList;
    private array $levels;
    private array $heuristics;
    private array $edgeCosts;

    public function __construct() {
        $this->adjacencyList = [];
        $this->levels = [];
        $this->heuristics = [];
        $this->edgeCosts = [];
    }

    public function addVertex(string $vertex, int $level = -1, float $heuristic = 0.0): void {
        if (!isset($this->adjacencyList[$vertex])) {
            $this->adjacencyList[$vertex] = [];
            $this->levels[$vertex] = $level;
            $this->heuristics[$vertex] = $heuristic;
            $this->edgeCosts[$vertex] = [];
        }
    }

    public function addEdge(string $from, string $to, float $cost = 1.0): void {
        if (!isset($this->adjacencyList[$from]) || !isset($this->adjacencyList[$to])) {
            throw new InvalidArgumentException("Both vertices must exist in the graph.");
        }

        if (!isset($this->edgeCosts[$from])) {
            $this->edgeCosts[$from] = [];
        }

        if (!in_array($to, $this->adjacencyList[$from])) {
            $this->adjacencyList[$from][] = $to;
            $this->edgeCosts[$from][$to] = $cost;
        }
    }

    public function getEdgeCost(string $from, string $to): float {
        if (!isset($this->edgeCosts[$from][$to])) {
            throw new InvalidArgumentException("No edge exists between $from and $to");
        }
        return $this->edgeCosts[$from][$to];
    }

    public function greedySearch(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        $path = [];
        $currentVertex = $start;

        // Keep going until we reach the goal
        while ($currentVertex !== $goal) {
            // Add current vertex to path
            $path[] = [
                'vertex' => $currentVertex,
                'level' => $this->levels[$currentVertex],
                'heuristic' => $this->heuristics[$currentVertex]
            ];

            // Get all neighbors of current vertex
            $neighbors = $this->adjacencyList[$currentVertex];

            if (empty($neighbors)) {
                return null; // Dead end
            }

            // Find neighbor with lowest heuristic value
            $bestNeighbor = null;
            $bestHeuristic = PHP_FLOAT_MAX;

            foreach ($neighbors as $neighbor) {
                $h = $this->heuristics[$neighbor];
                if ($h < $bestHeuristic) {
                    $bestHeuristic = $h;
                    $bestNeighbor = $neighbor;
                }
            }

            // If we can't find a better neighbor, we're stuck
            if ($bestNeighbor === null) {
                return null;
            }

            // Move to the best neighbor
            $currentVertex = $bestNeighbor;
        }

        // Add the goal vertex to complete the path
        $path[] = [
            'vertex' => $goal,
            'level' => $this->levels[$goal],
            'heuristic' => $this->heuristics[$goal]
        ];

        return $path;
    }

    public function aStarTreeSearch(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        // Priority queue implemented as array with state information
        $openSet = [[
            'vertex' => $start,
            'gScore' => 0.0,
            'fScore' => $this->heuristics[$start],
            'path' => [
                [
                    'vertex' => $start,
                    'level' => $this->levels[$start],
                    'heuristic' => $this->heuristics[$start]
                ]
            ]
        ]];

        while (!empty($openSet)) {
            // Find node in openSet with lowest fScore
            $currentIndex = 0;
            $currentFScore = $openSet[0]['fScore'];

            for ($i = 1; $i < count($openSet); $i++) {
                if ($openSet[$i]['fScore'] < $currentFScore) {
                    $currentIndex = $i;
                    $currentFScore = $openSet[$i]['fScore'];
                }
            }

            $current = $openSet[$currentIndex];
            $currentVertex = $current['vertex'];

            // Check if we've reached the goal
            if ($currentVertex === $goal) {
                return $current['path'];
            }

            // Remove current from openSet
            array_splice($openSet, $currentIndex, 1);

            // Explore all neighbors
            foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
                // Calculate g score for this path
                $tentativeGScore = $current['gScore'] + $this->getEdgeCost($currentVertex, $neighbor);

                // Calculate f score (g score + heuristic)
                $fScore = $tentativeGScore + $this->heuristics[$neighbor];

                // Create new path
                $newPath = $current['path'];
                $newPath[] = [
                    'vertex' => $neighbor,
                    'level' => $this->levels[$neighbor],
                    'heuristic' => $this->heuristics[$neighbor]
                ];

                // Add new state to openSet
                $openSet[] = [
                    'vertex' => $neighbor,
                    'gScore' => $tentativeGScore,
                    'fScore' => $fScore,
                    'path' => $newPath
                ];
            }
        }

        return null; // No path found
    }

    public function aStarGroupSearch(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        $openSet = [$start];
        $cameFrom = [];
        $gScore = [$start => 0.0];
        $fScore = [$start => $this->heuristics[$start]];

        while (!empty($openSet)) {
            // Find node in openSet with lowest fScore
            $current = $openSet[0];
            $currentFScore = $fScore[$current];
            foreach ($openSet as $node) {
                if ($fScore[$node] < $currentFScore) {
                    $current = $node;
                    $currentFScore = $fScore[$node];
                }
            }

            if ($current === $goal) {
                // Reconstruct path
                $path = [$current];
                while (isset($cameFrom[$current])) {
                    $current = $cameFrom[$current];
                    array_unshift($path, $current);
                }
                return $path;
            }

            // Remove current from openSet
            $openSet = array_values(array_diff($openSet, [$current]));

            // Explore neighbors
            foreach ($this->adjacencyList[$current] as $neighbor) {
                $tentativeGScore = $gScore[$current] + $this->getEdgeCost($current, $neighbor);

                if (!isset($gScore[$neighbor]) || $tentativeGScore < $gScore[$neighbor]) {
                    $cameFrom[$neighbor] = $current;
                    $gScore[$neighbor] = $tentativeGScore;
                    $fScore[$neighbor] = $tentativeGScore + $this->heuristics[$neighbor];

                    if (!in_array($neighbor, $openSet)) {
                        $openSet[] = $neighbor;
                    }
                }
            }
        }

        return null; // No path found
    }

    public function beamSearch(string $start, string $goal, int $beamWidth = 2): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        // Initialize beam with start vertex
        $beam = [[
            'vertex' => $start,
            'path' => [
                [
                    'vertex' => $start,
                    'level' => $this->levels[$start],
                    'heuristic' => $this->heuristics[$start]
                ]
            ]
        ]];

        $visited = [$start => true];

        while (!empty($beam)) {
            $candidates = [];

            // Generate all possible next states from current beam
            foreach ($beam as $state) {
                $currentVertex = $state['vertex'];

                // Check if we've reached the goal
                if ($currentVertex === $goal) {
                    return $state['path'];
                }

                // Get all neighbors of current vertex
                foreach ($this->adjacencyList[$currentVertex] as $neighbor) {
                    // Skip if we've already visited this vertex
                    if (isset($visited[$neighbor])) {
                        continue;
                    }

                    $newPath = $state['path'];
                    $newPath[] = [
                        'vertex' => $neighbor,
                        'level' => $this->levels[$neighbor],
                        'heuristic' => $this->heuristics[$neighbor]
                    ];

                    $candidates[] = [
                        'vertex' => $neighbor,
                        'path' => $newPath,
                        'heuristic' => $this->heuristics[$neighbor]
                    ];

                    $visited[$neighbor] = true;
                }
            }

            if (empty($candidates)) {
                return null; // No path found
            }

            // Sort candidates by heuristic value (ascending)
            usort($candidates, function($a, $b) {
                return $a['heuristic'] <=> $b['heuristic'];
            });

            // Keep only the k best candidates (beam width)
            $beam = array_slice($candidates, 0, $beamWidth);
        }

        return null; // No path found
    }

    public function idaStarSearch(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        // Initialize the bound as the heuristic value of the start node
        $bound = $this->heuristics[$start];

        // Initial path contains only the start node
        $path = [
            [
                'vertex' => $start,
                'level' => $this->levels[$start],
                'heuristic' => $this->heuristics[$start]
            ]
        ];

        while (true) {
            // Search with current bound
            $result = $this->idaStarRecursive($path, 0, $bound, $goal);

            if (is_array($result)) {
                // Path found
                return $result;
            }

            if ($result === PHP_FLOAT_MAX) {
                // No path exists
                return null;
            }

            // Update bound to the minimum f-value that exceeded current bound
            $bound = $result;
        }
    }

    private function idaStarRecursive(array $path, float $g, float $bound, string $goal): array|float {
        $current = $path[count($path) - 1]['vertex'];
        $f = $g + $this->heuristics[$current];

        // If f exceeds bound, return f as the new minimum bound
        if ($f > $bound) {
            return $f;
        }

        // If goal is reached, return the path
        if ($current === $goal) {
            return $path;
        }

        $min = PHP_FLOAT_MAX;

        // Explore all neighbors
        foreach ($this->adjacencyList[$current] as $neighbor) {
            // Check if neighbor is already in path (avoid cycles)
            $inPath = false;
            foreach ($path as $node) {
                if ($node['vertex'] === $neighbor) {
                    $inPath = true;
                    break;
                }
            }
            if ($inPath) {
                continue;
            }

            // Add neighbor to path
            $path[] = [
                'vertex' => $neighbor,
                'level' => $this->levels[$neighbor],
                'heuristic' => $this->heuristics[$neighbor]
            ];

            // Recursively search from neighbor
            $result = $this->idaStarRecursive(
                $path,
                $g + $this->getEdgeCost($current, $neighbor),
                $bound,
                $goal
            );

            // Remove neighbor from path (backtrack)
            array_pop($path);

            // Process result
            if (is_array($result)) {
                // Path to goal found
                return $result;
            }

            // Update minimum bound if needed
            if ($result < $min) {
                $min = $result;
            }
        }

        return $min;
    }

    public function printPath(array $path, bool $showCost = true): void {
        $totalCost = 0;
        $previousVertex = null;

        foreach ($path as $step) {
            // Get current vertex name depending on path format
            $currentVertex = is_array($step) ? $step['vertex'] : $step;

            if ($showCost) {
                if ($previousVertex !== null) {
                    $edgeCost = $this->getEdgeCost($previousVertex, $currentVertex);
                    $totalCost += $edgeCost;
                    echo sprintf("Edge cost from %s to %s: %.1f\n",
                        $previousVertex,
                        $currentVertex,
                        $edgeCost
                    );
                }
            }

            echo sprintf("Node: %s (Level %d, h=%.1f)\n",
                $currentVertex,
                $this->levels[$currentVertex],
                $this->heuristics[$currentVertex]
            );

            $previousVertex = $currentVertex;
        }

        if ($showCost) {
            echo sprintf("\nTotal path cost: %.1f\n", $totalCost);
        }
    }

    public function printGraph(): void {
        foreach ($this->adjacencyList as $vertex => $neighbors) {
            $costs = array_map(function($neighbor) use ($vertex) {
                return sprintf("%s(%.1f)", $neighbor, $this->edgeCosts[$vertex][$neighbor]);
            }, $neighbors);

            echo sprintf("%s (Level %d, h=%.1f) -> %s\n",
                $vertex,
                $this->levels[$vertex],
                $this->heuristics[$vertex],
                implode(', ', $costs)
            );
        }
    }
}
```

</details>

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new InformedSearchGraph();

// Add vertices with their levels and heuristic values (h)
$graph->addVertex('S', 0, 7);  // Start node, h=7
$graph->addVertex('A', 1, 9);  // h=9
$graph->addVertex('D', 1, 5);  // h=5
$graph->addVertex('B', 2, 4);  // h=4
$graph->addVertex('E', 2, 3);  // h=3
$graph->addVertex('G', 3, 0);  // Goal node, h=0

// Add directed edges according to the diagram
$graph->addEdge('S', 'A');  // S -> A
$graph->addEdge('S', 'D');  // S -> D
$graph->addEdge('D', 'B');  // D -> B
$graph->addEdge('D', 'E');  // D -> E
$graph->addEdge('E', 'G');  // E -> G

// Perform greedy search from S to G
echo "Performing greedy search from S to G:\n\n";
$path = $graph->greedySearch('S', 'G');

if ($path !== null) {
    echo "Path found:\n";
    $graph->printPath($path);
} else {
    echo "No path found!\n";
}
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
