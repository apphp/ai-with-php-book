# Bidirectional Search (BDS)

Bidirectional Search (BDS) is an efficient graph traversal algorithm that conducts two simultaneous searches: one starting from the initial state (forward search) and the other from the goal state (backward search). These searches progress until their respective search trees intersect, signaling that a solution path has been found. By effectively replacing a single large search space with two smaller subgraphs, BDS minimizes the computational overhead, making it an attractive option for navigating vast graphs.

BDS is versatile and can utilize search strategies such as Breadth-First Search (BFS), Depth-First Search (DFS), or Depth-Limited Search (DLS), depending on the problem requirements and resource constraints.

### Key Characteristics

1. **Dual Search Process**: BDS simultaneously initiates a forward search from the starting node and a backward search from the goal node.
2. **Intersection-Based Termination**: The algorithm halts when the two search trees intersect, signifying that a valid path has been found.
3. **Graph Splitting**: Instead of exploring the entire graph in one direction, it reduces the search space by effectively dividing it into two smaller subgraphs.
4. **Customizable Search Techniques**: Techniques such as BFS and DFS can be employed in either or both directions, depending on the nature of the problem.
5. **Symmetry**: BDS assumes that traversal is possible in both forward and backward directions, which may not be suitable for all graph structures.

**Complexity**

Searches simultaneously from the initial state and the goal state, meeting in the middle. It can significantly reduce time complexity.

* Time Complexity: $$O(b^\frac{d}{2})$$.
* Space Complexity: $$O(b^\frac{d}{2})$$.
* Use Case: Finding the shortest path in undirected graphs.

### Advantages

1. **Faster Search**: By splitting the problem into two smaller subgraphs, BDS significantly reduces the number of nodes that need to be expanded, leading to quicker results.
2. **Memory Efficiency**: Smaller subgraphs require less memory compared to a single expansive search tree.
3. **Scalability**: BDS is particularly useful for extremely large graphs where a single-direction search would be computationally prohibitive.
4. **Reduced Node Expansion Costs**: In scenarios where expanding nodes is resource-intensive, BDS limits the total number of expansions, optimizing resource usage.

### Disadvantages

1. **Complex Implementation**: Managing two simultaneous search processes and ensuring their intersection is non-trivial, requiring careful implementation.
2. **Predefined Goal State**: BDS necessitates prior knowledge of the goal state, which might not always be available in certain problem domains.
3. **Intersection Challenges**: Efficiently identifying the intersection between two search trees can be difficult, potentially increasing the algorithm’s runtime.
4. **Symmetry Requirement**: The algorithm assumes that traversal is feasible in both forward and backward directions, limiting its applicability in asymmetric graphs.

Bidirectional Search is a powerful and efficient algorithm for traversing large graphs, leveraging the synergy of dual search trees to minimize computational effort. While its implementation complexity and dependency on goal-state knowledge pose challenges, the algorithm’s benefits in speed and memory efficiency make it a compelling choice for solving a variety of graph-based problems.

### Example

In the search tree below, the bidirectional search algorithm is applied. This approach splits the graph or tree into two sub-graphs. One search begins from the starting node (node **A**) and proceeds forward, while the other starts from the goal node (node **O**) and moves backward.

The algorithm concludes at node **H**, where the two searches intersect.

<div align="left"><figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Complexity, **Completeness** and Optimality

**Completeness**:&#x20;

Bidirectional search is complete when BFS is used for both forward and backward searches.

**Time Complexity**:&#x20;

In an ideal scenario, where the search trees meet in the middle of the search space, the time complexity of BDS is significantly better than a single-directional search.

1. **Single-Directional Search:** For algorithms like Breadth-First Search (BFS) applied to an unweighted graph, the time complexity is $$O(b^d)$$ , where:
   1. $$b$$ is the branching factor (average number of child nodes per node).
   2. $$d$$ is the depth of the solution from the root.
2. **Bidirectional Search**: By splitting the search into two halves (forward and backward), the depth each search needs to explore is approximately $$d/2$$. \
   Thus, the time complexity becomes: $$O(2 \cdot b^{d/2}) = O(b^{d/2})$$\
   This is a significant improvement over $$O(b^d)$$, especially for large d, as the exponential term grows much slower.

**Space Complexity**:&#x20;

Bidirectional Search requires maintaining two search trees in memory: one for the forward search and another for the backward search.

1. **Single-Directional Search**: Space complexity for BFS is $$O(b^d)$$, as it needs to store all nodes at the current level before moving to the next.
2. **Bidirectional Search**: Each of the two search trees has a space complexity of $$O(b^{d/2})$$. Combined, the total space complexity is approximately: $$O(2 \cdot b^{d/2}) = O(b^{d/2})$$

**Optimality**:&#x20;

Bidirectional search is optimal.

**Practical Considerations**:

While the theoretical complexities make Bidirectional Search appealing, practical factors like the following can affect performance:

* The need for efficient data structures to detect intersections between search trees.
* Symmetry in the graph for effective backward traversal.
* The overhead of managing two concurrent searches.

In summary, Bidirectional Search has a better theoretical complexity than single-directional search in terms of both time and space, particularly for large search depths. However, its practical performance depends heavily on implementation and problem-specific constraints.

### Iterative Bidirectional Search with PHP

In PHP  it can be written as a class `UninformedSearchGraph` with implementation of a set of graph operations.

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new UninformedSearchGraph();

// Add all vertices with their respective levels to show the tree structure
$vertices = [
    'A' => 0,  // Root level
    'B' => 1, 'E' => 1,  // Level 1
    'C' => 2, 'D' => 2, 'F' => 1, 'G' => 2,  // Level 2
    'H' => 3,  // Intersection node - Level 3
    'I' => 2, // Level 2 from the other direction
    'J' => 1, 'K' => 1,  // Level 1 from the other direction
    'L' => 2, 'M' => 2, 'N' => 2, 'O' => 2  // Level 2 from the other direction
];

// Add vertices with their levels
foreach ($vertices as $vertex => $level) {
    $graph->addVertex($vertex, $level);
}

// Add edges to create the exact graph structure
// Left side (from root A)
$graph->addEdge('A', 'E');
$graph->addEdge('E', 'B');
$graph->addEdge('E', 'G');
$graph->addEdge('F', 'C');
$graph->addEdge('F', 'D');
$graph->addEdge('F', 'G');

// Center (where searches will meet)
$graph->addEdge('G', 'H');

// Right side (from goal O)
$graph->addEdge('H', 'I');
$graph->addEdge('I', 'J');
$graph->addEdge('I', 'K');
$graph->addEdge('J', 'L');
$graph->addEdge('J', 'M');
$graph->addEdge('K', 'N');
$graph->addEdge('K', 'O');

// Perform bidirectional search from A to O
echo "Bidirectional Search from A to O:\n";
echo "--------------------------------\n";
$result = $graph->bds('A', 'O');

// Print detailed search results
$graph->printBdsPath($result);

// Let's analyze the intersection at node H
if ($result['success'] && $result['intersectionVertex'] === 'H') {
    echo "\nSearch Analysis:\n";
    echo "----------------\n";

    // Forward search path to H
    echo "Forward search path (A → H):\n";
    foreach ($result['forwardExplored'] as $node) {
        if ($node['vertex'] === 'H') {
            echo "→ Reached intersection node H\n";
            break;
        }
        echo "→ {$node['vertex']} (Level {$node['level']})\n";
    }

    // Backward search path to H
    echo "\nBackward search path (O → H):\n";
    foreach ($result['backwardExplored'] as $node) {
        if ($node['vertex'] === 'H') {
            echo "→ Reached intersection node H\n";
            break;
        }
        echo "→ {$node['vertex']} (Level {$node['level']})\n";
    }

    // Complete path
    echo "\nComplete path found (A → O through H):\n";
    foreach ($result['path'] as $node) {
        echo "→ {$node['vertex']} (Level {$node['level']})";
        if ($node['vertex'] === 'H') {
            echo " [INTERSECTION]";
        }
        echo "\n";
    }
}

// Verify we have a valid path through H
echo "\nPath verification:\n";
echo "-----------------\n";
if ($result['success']) {
    echo "✓ Path successfully found\n";
    echo "✓ Intersection occurred at node H\n";
    echo "✓ Total nodes in path: " . count($result['path']) . "\n";
} else {
    echo "✗ No valid path found\n";
}
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
