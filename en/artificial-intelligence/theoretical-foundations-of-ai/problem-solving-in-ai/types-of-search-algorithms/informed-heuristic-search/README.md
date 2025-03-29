# Informed (Heuristic) Search

Informed search algorithms utilize problem-specific knowledge to guide the search, making them more efficient than uninformed methods. This knowledge, known as a heuristic, estimates the cost to reach the goal from a given state. While heuristics do not guarantee the best solution, they often provide good solutions within a reasonable timeframe.

### Key Characteristics of Informed Search

1. **Uses Heuristics**: Guides the search process based on domain knowledge.
2. **More Efficient**: Requires fewer node expansions compared to uninformed methods.
3. **Handles Complex Problems**: Solves problems that are computationally infeasible for uninformed search.

### Types of Informed Search Algorithms

1. **Greedy Search**
2. **A\* Tree Search**
3. **A\* Graph Search**
4. **Iterative Deepening A\* (IDA)**
5. **Beam Search**
6. **Hill Climbing Algorithm**
7. **Simulated Annealing**
8. **Local Beam Search**
9. **Genetic Algorithms**
10. **Tabu Search**

Let's take a closer look at all these types.

### Informed Search Algorithms with PHP

In PHP it can be written as a class `InformedSearchGraph` with implementation of a set of graph operations.

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

    /**
     * Simple Hill Climbing: Takes the first neighbor that improves the current state.
     * Faster but may miss better solutions.
     */
    public function simpleHillClimbing(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        $currentVertex = $start;
        $path = [[
            'vertex' => $start,
            'level' => $this->levels[$start],
            'heuristic' => $this->heuristics[$start]
        ]];

        $visited = [$start => true];

        while ($currentVertex !== $goal) {
            $neighbors = $this->adjacencyList[$currentVertex];
            if (empty($neighbors)) {
                return null; // Dead end
            }

            // Find first better neighbor
            $betterFound = false;
            $currentHeuristic = $this->heuristics[$currentVertex];

            foreach ($neighbors as $neighbor) {
                if (isset($visited[$neighbor])) {
                    continue;
                }

                if ($this->heuristics[$neighbor] < $currentHeuristic) {
                    $currentVertex = $neighbor;
                    $visited[$neighbor] = true;
                    $path[] = [
                        'vertex' => $neighbor,
                        'level' => $this->levels[$neighbor],
                        'heuristic' => $this->heuristics[$neighbor]
                    ];
                    $betterFound = true;
                    break;
                }
            }

            if (!$betterFound) {
                return null; // Local maximum reached
            }
        }

        return $path;
    }

    /**
     * Steepest Ascent Hill Climbing: Evaluates all neighbors and selects the one
     * that provides the maximum improvement. More thorough but slower.
     */
    public function steepestAscentHillClimbing(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        $currentVertex = $start;
        $path = [[
            'vertex' => $start,
            'level' => $this->levels[$start],
            'heuristic' => $this->heuristics[$start]
        ]];

        $visited = [$start => true];

        while ($currentVertex !== $goal) {
            $neighbors = $this->adjacencyList[$currentVertex];
            if (empty($neighbors)) {
                return null; // Dead end
            }

            // Find the neighbor with maximum improvement
            $bestNeighbor = null;
            $bestImprovement = 0;
            $currentHeuristic = $this->heuristics[$currentVertex];

            foreach ($neighbors as $neighbor) {
                if (isset($visited[$neighbor])) {
                    continue;
                }

                $improvement = $currentHeuristic - $this->heuristics[$neighbor];
                if ($improvement > $bestImprovement) {
                    $bestImprovement = $improvement;
                    $bestNeighbor = $neighbor;
                }
            }

            if ($bestNeighbor === null) {
                return null; // Local maximum reached
            }

            $currentVertex = $bestNeighbor;
            $visited[$currentVertex] = true;
            $path[] = [
                'vertex' => $currentVertex,
                'level' => $this->levels[$currentVertex],
                'heuristic' => $this->heuristics[$currentVertex]
            ];
        }

        return $path;
    }

    /**
     * Stochastic Hill Climbing: Randomly selects among the better neighbors,
     * with probability proportional to the amount of improvement.
     * Can escape some local maxima but may take longer to converge.
     */
    public function stochasticHillClimbing(string $start, string $goal): ?array {
        if (!isset($this->adjacencyList[$start]) || !isset($this->adjacencyList[$goal])) {
            throw new InvalidArgumentException("Both start and goal vertices must exist in the graph.");
        }

        $currentVertex = $start;
        $path = [[
            'vertex' => $start,
            'level' => $this->levels[$start],
            'heuristic' => $this->heuristics[$start]
        ]];

        $visited = [$start => true];
        $maxAttempts = 100; // Prevent infinite loops
        $attempts = 0;

        while ($currentVertex !== $goal && $attempts < $maxAttempts) {
            $neighbors = $this->adjacencyList[$currentVertex];
            if (empty($neighbors)) {
                return null; // Dead end
            }

            // Collect better neighbors and their improvements
            $candidates = [];
            $totalImprovement = 0;
            $currentHeuristic = $this->heuristics[$currentVertex];

            foreach ($neighbors as $neighbor) {
                if (isset($visited[$neighbor])) {
                    continue;
                }

                $improvement = $currentHeuristic - $this->heuristics[$neighbor];
                if ($improvement > 0) {
                    $candidates[] = [
                        'vertex' => $neighbor,
                        'improvement' => $improvement
                    ];
                    $totalImprovement += $improvement;
                }
            }

            if (empty($candidates)) {
                return null; // Local maximum reached
            }

            // Randomly select a neighbor, weighted by improvement
            $random = mt_rand() / mt_getrandmax() * $totalImprovement;
            $sum = 0;
            $selectedNeighbor = null;

            foreach ($candidates as $candidate) {
                $sum += $candidate['improvement'];
                if ($sum >= $random) {
                    $selectedNeighbor = $candidate['vertex'];
                    break;
                }
            }

            if ($selectedNeighbor === null) {
                $selectedNeighbor = $candidates[array_key_last($candidates)]['vertex'];
            }

            $currentVertex = $selectedNeighbor;
            $visited[$currentVertex] = true;
            $path[] = [
                'vertex' => $currentVertex,
                'level' => $this->levels[$currentVertex],
                'heuristic' => $this->heuristics[$currentVertex]
            ];

            $attempts++;
        }

        return $attempts < $maxAttempts ? $path : null;
    }

    public function searchAnalysis($searchResult, bool $showCost = true): void {
        if ($searchResult === null) {
            echo "No path found!\n";
            return;
        }

        $totalCost = 0;

        echo "Path sequence: ";
        $pathSequence = array_map(function ($node) {
            return $node['vertex'];
        }, $searchResult);
        echo implode(" -> ", $pathSequence) . "\n";

        echo "\nPath analysis:\n";
        $lastIndex = 0;
        $lastVertex = null;
        foreach ($pathSequence as $index => $vertex) {
            if ($index > 0) {
                $prevVertex = $pathSequence[$index - 1];
                $cost = $this->getEdgeCost($prevVertex, $vertex);
                $totalCost += $cost;
                echo sprintf("Step %d: %s (level %d, h=%.1f) -> %s (level %d, h=%.1f): cost: %.1f\n",
                    $index,
                    $prevVertex,
                    $this->levels[$prevVertex],
                    $this->heuristics[$prevVertex],
                    $vertex,
                    $this->levels[$vertex],
                    $this->heuristics[$vertex],
                    $cost,
                //$searchResult[$index]['heuristic']
                );
            }
            $lastIndex++;
            $lastVertex = $vertex;
        }

        echo sprintf("Step %d: %s (level %d, h=%.1f)\n",
            $lastIndex,
            $lastVertex,
            $this->levels[$vertex],
            $this->heuristics[$vertex]
        );

        if ($showCost) {
            echo sprintf("\nTotal path cost: %.1f\n", $totalCost);
        }
    }

    public function getAdjacencyList(): array {
        return $this->adjacencyList;
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

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
