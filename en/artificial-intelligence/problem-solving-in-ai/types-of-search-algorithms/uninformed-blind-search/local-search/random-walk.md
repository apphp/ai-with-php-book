# Random Walk Search

The Random Walk Search algorithm is a fundamental search strategy where a search agent randomly selects and moves to a neighboring node without maintaining a history of past states. This approach is often used in scenarios where structured search methods are either infeasible or inefficient due to an unknown or highly complex search space.

### Key Characteristics

1. **Stochastic Nature:**\
   The search progresses randomly, choosing one of the available neighbors at each step without a deterministic pattern.
2. **No Backtracking:**\
   Unlike systematic search algorithms, Random Walk does not revisit past nodes explicitly unless reached randomly again.
3. **Application in Exploration:**\
   Useful in cases where heuristic or exhaustive search strategies are not practical, such as when dealing with unknown graphs or networks.

**Complexity**

* **Time Complexity:** Highly variable, dependent on the structure of the search space and the probability of reaching a goal node.
* **Space Complexity:** $$O(1)$$, as only the current state is stored, making it extremely memory efficient.

**Use** **Case**

* Exploring large or infinite state spaces where exhaustive search is computationally prohibitive.
* Simulating diffusion processes or random behavior in probabilistic models.
* Generating diverse solutions in optimization problems.

### Advantages

1. **Memory Efficiency:**\
   Requires minimal memory as it does not store visited nodes.
2. **Simplicity:**\
   Easy to implement with minimal computational overhead.
3. **Works in Unknown Environments:**\
   Suitable for exploration where no prior knowledge of the structure is available.

### Disadvantages

1. **Inefficiency:**\
   Can take an extremely long time to find a solution, especially in large or complex graphs.
2. **Non-Optimal Solutions:**\
   Does not guarantee the shortest or best solution, as movements are random.
3. **No Guarantee of Termination:**\
   In some cases, the search may not converge to a solution within a practical timeframe.

Random Walk Search is a simple yet powerful approach for exploring unknown search spaces. While it is highly memory-efficient and easy to implement, it lacks efficiency and guarantees. It is best used in exploratory tasks, probabilistic simulations, or as a component in hybrid search strategies where exhaustive or heuristic-based searches are impractical.

### Example

In the search tree below, the flow of the random walk search can be following this order:

$$S→A→C→D→B→I→J$$

....

### Complexity, **Completeness** and Optimality

**Completeness:**

The algorithm is **incomplete** as it does not guarantee finding the goal, especially in large graphs with dead-ends.

**Time Complexity:**

In the worst case, it may take exponential time or even fail to find the goal.

**Space Complexity:**

$$O(1)$$ as only the current state is stored.

**Optimality:**

Random Walk does not guarantee the shortest or most efficient path to the goal.

### Random Walk Search with PHP

In PHP  it can be written as a class `UninformedSearchGraph` with implementation of a set of graph operations.

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new UninformedSearchGraph();

// Add all vertices with their respective levels
$graph->addVertex('S', 0);  // Start node at level 0
$graph->addVertex('A', 1);
$graph->addVertex('B', 1);
$graph->addVertex('C', 2);
$graph->addVertex('D', 2);
$graph->addVertex('I', 2);
$graph->addVertex('J', 2);
$graph->addVertex('E', 3);
$graph->addVertex('F', 3);
$graph->addVertex('G', 3);
$graph->addVertex('H', 3);

// Add edges according to the graph structure
$graph->addEdge('S', 'A');
$graph->addEdge('S', 'B');
$graph->addEdge('A', 'C');
$graph->addEdge('A', 'D');
$graph->addEdge('B', 'I');
$graph->addEdge('B', 'J');
$graph->addEdge('C', 'E');
$graph->addEdge('C', 'F');
$graph->addEdge('D', 'G');
$graph->addEdge('I', 'H');

// Try DLS with different depth limits
$depths = [1, 2, 3];

foreach ($depths as $maxDepth) {
    echo "\nTrying DLS with max depth = $maxDepth to find node 'J':\n";
    $result = $graph->dls('S', $maxDepth, 'J');

    echo $result['found']
        ? "✓ Target 'J' found within depth limit!\n"
        : "✗ Target 'J' not found within depth limit of {$result['maxDepth']}\n";

    echo "Path explored:\n";
    foreach ($result['path'] as $node) {
        echo sprintf("  Node: %s (Level %d, Search Depth %d)\n",
            $node['vertex'],
            $node['level'],
            $node['depth']
        );
    }
}
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

####
