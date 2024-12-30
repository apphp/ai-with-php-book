# A\* Graph Search

A\* Graph Search is an enhancement of the A\* Tree Search algorithm, designed to optimize its efficiency by addressing a key limitation: the re-exploration of nodes. In tree search, the same node can be expanded multiple times across different branches, wasting time and computational resources.&#x20;

A\* Graph Search mitigates this issue by introducing a critical rule: a node is not expanded more than once. This improvement allows the algorithm to explore paths more efficiently while retaining the benefits of A\*’s heuristic-based approach.

### Key Characteristics

1. **Node Tracking**:\
   A\* Graph Search maintains a record of explored nodes, ensuring that no node is revisited. This is typically implemented using a closed list to store processed nodes.
2.  **Optimality**:\
    A\* Graph Search remains optimal if the heuristic function used is consistent. Consistency means that the estimated cost of moving from one node $$A$$ to another $$B$$ via the heuristic $$h$$

    satisfies the condition: $$h(A) - h(B) \leq g(A \to B)$$\
    Here, $$g(A \to B)$$ represents the actual cost between nodes $$A$$ and $$B$$.
3. **Heuristic Function**:\
   The heuristic guides the search by estimating the cost from the current node to the goal. A consistent heuristic ensures both optimality and efficiency in finding the shortest path.
4. **Pathfinding**:\
   The algorithm works similarly to A\* Tree Search but avoids redundant work by skipping previously explored nodes, allowing for faster convergence to the solution.

### Advantages

* **Efficiency**:\
  By avoiding re-expansion of nodes, A\* Graph Search reduces computational overhead and speeds up the search process.
* **Optimality**:\
  With a consistent heuristic, A\* Graph Search guarantees finding the least-cost path to the goal.
* **Scalability**:\
  Suitable for complex graphs with many interconnected nodes, as it avoids unnecessary exploration.

### Disadvantages

* **Memory Usage**:\
  Maintaining a record of explored nodes (closed list) requires additional memory, which can be a limitation for very large graphs.
* **Heuristic Sensitivity**:\
  The algorithm’s performance and optimality heavily depend on the quality and consistency of the heuristic function.
* **Setup Complexity**:\
  Implementing node tracking and ensuring heuristic consistency may add complexity to the algorithm’s setup.

### Example

Find the path to reach from S to G using A\* search.

<div align="left"><figure><img src="../../../../../.gitbook/assets/image (112).png" alt="" width="563"><figcaption></figcaption></figure></div>

**Solution**: The algorithm is applied as follows:

1\. Begin at the start node .

2\. Expand nodes using the heuristic and path cost, while tracking explored nodes to prevent re-expansion.

3\. Continue until the goal node is reached.

Let's explore the work of the search:

<table><thead><tr><th width="217">Path</th><th>h(x)</th><th>g(x)</th><th>f(x)</th></tr></thead><tbody><tr><td>S</td><td>7</td><td>0</td><td>7</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td>S -> A</td><td>9</td><td>3</td><td>12</td></tr><tr><td>S -> D                         ✓</td><td>5</td><td>2</td><td>7</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td>S -> D -> B                 ✓</td><td>4</td><td>2 + 1 = 3</td><td>7</td></tr><tr><td>S -> D -> E</td><td>3</td><td>2 + 4 = 6</td><td>9</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td>S -> D -> B -> C         ✓</td><td>2</td><td>3 + 2 = 5</td><td>7</td></tr><tr><td>S -> D -> B -> E         ✓</td><td>3</td><td>3 + 1 = 4</td><td>7</td></tr><tr><td> </td><td> </td><td> </td><td> </td></tr><tr><td>S -> D -> B -> C -> G</td><td>0</td><td>5 + 4 = 9</td><td>9</td></tr><tr><td><strong>S -> D -> B -> E -> G</strong> ✓</td><td>0</td><td>4 + 3 = 7</td><td>7</td></tr></tbody></table>

**Result**:

• Path: the sequence of nodes visited during traversal will be: $$S→D→B→E→G$$

• Cost: 7

### Complexity, **Completeness** and Optimality

#### Time Complexity:

A\* Graph Search explores nodes once, which reduces redundant computations. However, its worst-case complexity remains O(b^d), where b is the branching factor and d is the depth of the optimal solution. The actual runtime can be much lower in practical applications.

#### Space Complexity:

It requires space to store all nodes in the graph, including the open list (nodes yet to be explored) and the closed list (already explored nodes). This makes it memory-intensive, especially for large graphs.

#### Completeness:

A\* Graph Search is guaranteed to find a solution if one exists, as long as the graph is finite and the heuristic is consistent.

#### Optimality:

Optimality is ensured when the heuristic is consistent, meaning it never overestimates the actual cost to reach the goal. This property guarantees that the first path found to the goal is the least-cost path.

### Conclusion

A\* Graph Search is a powerful algorithm for pathfinding and graph traversal, building upon the strengths of A\* Tree Search while eliminating inefficiencies. Its ability to avoid redundant work makes it an essential tool for solving problems in robotics, navigation, gaming, and other domains requiring optimal pathfinding. However, its reliance on memory and heuristic quality must be considered during implementation.

### A\* Graph Search with PHP

In PHP  it can be written as a class `InformedSearchGraph` with implementation of a set of graph operations.

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new InformedSearchGraph();

// Add vertices with their levels and heuristic values
$graph->addVertex('S', 0, 7);  // Start node
$graph->addVertex('A', 1, 9);
$graph->addVertex('D', 1, 5);
$graph->addVertex('B', 2, 4);
$graph->addVertex('E', 2, 3);
$graph->addVertex('C', 3, 2);
$graph->addVertex('G1', 4, 0); // First G node (from C)
$graph->addVertex('G2', 3, 0); // Second G node (from E)
$graph->addVertex('G', 4, 0); // Third G node (from E)

// Add edges with their costs
$graph->addEdge('S', 'A', 3);
$graph->addEdge('S', 'D', 2);
$graph->addEdge('D', 'B', 1);
$graph->addEdge('D', 'E', 4);
$graph->addEdge('B', 'C', 2);
$graph->addEdge('B', 'E', 1);
$graph->addEdge('C', 'G1', 4); // Path to first G
$graph->addEdge('E', 'G2', 3); // Path to second G
$graph->addEdge('E', 'G', 3); // Path to third G (the one highlighted in orange)

// Find path using A* Group search to G3 (the highlighted goal node)
echo "Performing A* Group Search from S to G:\n";
echo "---------------------------------------\n\n";

$path = $graph->aStarGroupSearch('S', 'G');
if ($path !== null) {
    $graph->printPath($path);
} else {
    echo "No path found!\n";
}
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
