# Depth-Limited Search (DLS)

The Depth-Limited Search (DLS) algorithm is an extension of the Depth-First Search (DFS) algorithm, designed to address the challenge of infinite paths in certain problem spaces. DLS introduces a predetermined depth limit to the search process, treating nodes at this limit as though they have no successors. By constraining the depth, DLS avoids the pitfalls of exploring infinite paths while maintaining the advantages of depth-first traversal. This makes it a valuable tool in solving problems with large or potentially unbounded search spaces, provided a suitable depth limit is chosen.

### Key Characteristics

1. **Depth Limitation**: \
   The algorithm restricts the search depth to a specified limit, ensuring that nodes beyond this depth are not expanded.
2. **Failure Conditions**:
   1. **Standard Failure**: \
      Indicates that the problem has no solution within the explored search space.
   2. **Cutoff Failure**: \
      Signals that the solution exists but lies beyond the specified depth limit.
3. **Behavior with Cycles**: \
   By not revisiting nodes, DLS avoids infinite loops caused by cycles in the search graph, a common issue in classical DFS.
4. **Memory Efficiency**: \
   DLS requires less memory than algorithms like Breadth-First Search (BFS), as it does not need to store all levels of the search tree simultaneously.

**Complexity**

* **Time Complexity**: $$O(b^m)$$ - same as DFS.
* **Space Complexity**: $$O(b \times m)$$ - same as DFS.
* **Use Case**: Solving problems with known depth constraints.

### Advantages

1. **Memory Conservation**: \
   By limiting the depth, DLS reduces the memory requirements compared to BFS and Iterative Deepening Depth-First Search (IDDFS). It avoids the need to hold the entire search tree in memory, making it more efficient for certain problems.
2. **Cycle Handling**: \
   DLS naturally mitigates issues with infinite loops in graphs containing cycles, as it stops expanding nodes at the depth limit.
3. **Focused Exploration**: \
   The depth restriction ensures that unnecessary exploration of deeper levels is avoided, leading to faster processing in cases where the solution lies within the set limit.

### Disadvantages

1. **Incompleteness**: \
   DLS is not guaranteed to find a solution if one exists, especially if the depth limit is set too low.
2. **Non-Optimal Solutions**: \
   The algorithm may not produce the optimal solution when multiple solutions exist at different depths.
3. **Dependency on Depth Limit**: \
   The effectiveness of DLS heavily relies on choosing an appropriate depth limit. A limit set too low may miss the solution, while a limit set too high can result in unnecessary exploration, reducing efficiency.

Depth-Limited Search strikes a balance between the exhaustive exploration of BFS and the memory constraints of DFS. Its utility lies in its ability to prevent infinite exploration while maintaining a structured approach to problem-solving. However, careful consideration must be given to the choice of the depth limit to ensure effective results, making it a trade-off between completeness and resource efficiency.

### Example

In the search tree below, the flow of the depth-first search follows this order:

$$S→A→C→D→B→I→J$$

The algorithm starts at the root node **S** (depth 0). It explores node **A** (depth 1), then moves to **C** (depth 2), and its children **E** and **F** (depth 3), which are not expanded further due to the depth limit. After that it backtracks to A and explores D (depth 2). Once A’s children are explored, the algorithm backtracks to **S** and explores **B** (depth 1), then **I** (depth 2), and finally reaches the goal node **J** at depth 3, terminating the search successfully.

<div align="left"><figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

**Completeness**:

The DLS algorithm is complete if the solution exists within the specified depth limit.

**Time Complexity**:

The time complexity of the DLS algorithm is $$O(b^m)$$, where  b  represents the branching factor of the search tree, and $$m$$ is the depth limit.

**Space Complexity**:

The space complexity of the DLS algorithm is $$O(b \times m)$$, where  b  is the branching factor and $$m$$ is the depth limit.

**Optimality**:

As a variation of DFS, depth-limited search is not optimal, even when the depth limit ($$m$$) exceeds the depth of the shallowest solution ($$d$$).

### Depth-Limited Search with PHP

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
