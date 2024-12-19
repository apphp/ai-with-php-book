# Uniform Cost Search (UCS)

Uniform Cost Search (UCS) is a fundamental algorithm widely used in artificial intelligence for traversing weighted trees or graphs. It is designed to handle situations where each edge has a different cost, aiming to find the path to the goal node with the lowest cumulative cost. UCS achieves this by expanding nodes based on their path costs, starting from the root node. This chapter explores UCS in detail, highlighting its key characteristics, advantages, and disadvantages.

### Key Characteristics

1. **Weighted Graph/Tree Traversal**: \
   UCS is specifically designed for environments where edge weights vary, making it ideal for finding cost-optimal solutions in such scenarios.
2. **Optimal Path Guarantee**: \
   The algorithm always finds the path with the minimum cumulative cost, ensuring that the result is optimal.
3. **Priority Queue Implementation**:\
   UCS utilizes a priority queue to manage node expansion. Nodes with the lowest cumulative cost are given the highest priority for exploration.
4. **Comparison with Breadth-First Search (BFS)**:\
   If all edges have the same weight, UCS behaves identically to BFS, as the lowest cumulative cost corresponds to the shortest path in terms of steps.
5. **Path Cost-Driven Expansion**: \
   Unlike algorithms that minimize the number of steps, UCS focuses exclusively on minimizing the cumulative cost, even if it involves traversing more steps.

**Complexity**

UCS expands the node with the lowest path cost, ensuring an optimal solution if all costs are non-negative.

* **Time Complexity:** $$O(b^{1 + \frac{C^*}{\epsilon}})$$ _, where_ $$C*$$ is the cost of the optimal solution, $$\epsilon$$ is the minimum cost of any action, and $$b$$ is the branching factor. Then the number of steps is = $$1 + \frac{C^*}{\epsilon}$$, as we start from state 0 and end to $$\frac{C^*}{\epsilon}$$.
* **Space Complexity**: $$O(b^{1 + \frac{C^*}{\epsilon}})$$, the same logic is for space complexity.
* **Use Case**: Finding the cheapest path in a weighted graph.

### Advantages

1. **Optimal Solution**:\
   UCS guarantees an optimal solution by always exploring the least costly path first.
2. **Efficient Exploration for Small Weights**:\
   When edge weights are small, UCS effectively finds the shortest paths early in the process.
3. **Simplicity**:\
   The algorithm is relatively straightforward and accessible for a wide range of users, making it a go-to choice for many weighted graph problems.
4. **Completeness:**\
   UCS is a complete algorithm, meaning it will always find a solution if one exists, provided the graph is finite and edge weights are non-negative.

### Disadvantages

1. **Insensitive to Number of Steps**:\
   UCS does not account for the number of steps in a path. It solely focuses on path costs, which can lead to longer paths if they are less costly.
2. **Requirement for Known Edge Weights**:\
   The algorithm assumes that all edge weights are known at the start of the search, limiting its application in environments with dynamic or uncertain costs.
3. **Memory Intensive**:\
   UCS maintains a priority queue of discovered nodes, which can become a significant memory burden for large graphs. The memory requirements increase further as it stores the full path sequences.
4. **Vulnerability to Cycles**: \
   In graphs with cycles, UCS may explore paths with lower cumulative costs that do not lead to the goal, potentially causing inefficiencies or infinite loops.
5. **High Computational Overhead**:\
   For large graphs with numerous edges, the constant management of the priority queue can lead to substantial computational overhead.

Uniform Cost Search is a powerful and reliable algorithm for finding the lowest-cost path in weighted graphs and trees. Its optimality and completeness make it a preferred choice for many applications, especially in scenarios demanding cost-efficient solutions. However, its memory and computational demands, coupled with its reliance on known edge weights, highlight the importance of selecting UCS judiciously based on the problem’s nature and constraints.

### Example

The sequence of nodes visited during traversal will be:

$$S→A→D→G$$

The Cost: 6

<div align="left"><figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Complexity, **Completeness** and Optimality

**Completeness**:

Uniform-cost search is a complete algorithm, meaning it is guaranteed to find a solution if one exists.

**Time Complexity**:

If  represents the cost of the optimal solution and  is the smallest increment in path cost that brings the search closer to the goal, then the total number of steps required can be approximated as _. The  accounts for the initial state. Consequently, the worst-case time complexity of UCS is_ , where  is the branching factor.

**Space Complexity**:

The space complexity follows the same logic as the time complexity since UCS maintains a priority queue of all discovered nodes. Therefore, the worst-case space complexity is also .

**Optimality**:

Uniform-cost search is guaranteed to be optimal, as it always expands the path with the lowest cumulative cost, ensuring the most cost-efficient solution is found.

### Iterative Uniform Cost Search with PHP

In PHP  it can be written as a class `UninformedSearchGraph` with implementation of a set of graph operations.

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new UninformedSearchGraph();

// Add all vertices with their respective levels
$graph->addVertex('S', 0); // Starting node at level 0
$graph->addVertex('A', 1);
$graph->addVertex('B', 1);
$graph->addVertex('C', 2);
$graph->addVertex('D', 2);
$graph->addVertex('E', 3);
$graph->addVertex('F', 3);
$graph->addVertex('G', 4); // There are multiple G nodes in different levels, but we'll use the target G

// Add edges with their weights according to the diagram
$graph->addEdge('S', 'A', 1);
$graph->addEdge('S', 'B', 4);
$graph->addEdge('A', 'C', 3);
$graph->addEdge('A', 'D', 2);
$graph->addEdge('B', 'G', 5);
$graph->addEdge('C', 'E', 5);
$graph->addEdge('D', 'F', 4);
$graph->addEdge('D', 'G', 3);
$graph->addEdge('E', 'G', 5);

// Perform UCS from S to G
echo "UCS traversal starting from vertex 'S':\n";
$result = $graph->ucs('S', 'G');

// Print the result
echo "\nUCS Path Result:\n";
$graph->printUcsPath($result);

echo "\nThe output should show S -> A -> D -> G as the optimal path";
echo "\nwith a total cost of 1 + 2 + 3 = 6";
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
