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

<div align="left"><figure><img src="../../../../.gitbook/assets/image (36).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Complexity, **Completeness** and Optimality

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

In PHP  it can be written as a class `UninformedSearchGraph` with implementation of a set of graph operations.

**Example of Use:**

```php
// Create the graph and add vertices with their levels
$graph = new UninformedSearchGraph();

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
