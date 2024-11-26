# Bidirectional Search (BDS)

Uniform Cost Search (UCS) is a pathfinding algorithm widely used in artificial intelligence and graph theory to find the optimal path from a start node to a goal node in a weighted graph. It operates on the principle of expanding the least-cost node first, ensuring that the path found is the one with the minimum cumulative cost. UCS is a variant of Dijkstra’s algorithm and guarantees finding the shortest path when the cost of transitions between nodes is positive.

This makes UCS particularly suitable for problems where different paths have varying costs, such as routing in networks, robotic navigation, or planning tasks.

### Key Characteristics

1. **Priority Queue Implementation**:\
   UCS uses a priority queue (often implemented with a min-heap) to maintain the nodes to be explored. Nodes with the least cumulative cost are prioritized.
2. **Cost-Based Expansion**:\
   Instead of exploring nodes based on depth or breadth, UCS selects nodes based on their cumulative cost from the start node.
3. **Optimality**:\
   If all edge costs are non-negative, UCS is guaranteed to find the optimal solution.
4. **Complete Search**:\
   UCS will explore all possible paths to the goal node, ensuring it finds a solution if one exists.
5. **Uniform Treatment of Edges**:\
   Unlike algorithms that assume uniform costs, UCS explicitly accounts for edge weights, making it flexible for diverse scenarios.

**Complexity**

Searches simultaneously from the initial state and the goal state, meeting in the middle. It can significantly reduce time complexity.

* Time Complexity: $$O(b^\frac{d}{2})$$.
* Space Complexity: $$O(b^\frac{d}{2})$$.
* Use Case: Finding the shortest path in undirected graphs.

### Advantages

1. **Guaranteed Optimal Solution**:\
   UCS always finds the path with the minimum cost, making it ideal for applications requiring optimal solutions.
2. **Handles Weighted Graphs**:\
   Unlike simpler algorithms like Breadth-First Search (BFS), UCS can handle graphs with varying edge weights effectively.
3. **Flexible for Complex Problems**:\
   It is particularly useful in scenarios where paths have significantly different costs, such as transportation networks or resource allocation tasks.
4. **Theoretical Basis**:\
   UCS’s foundation in cost-based prioritization aligns with various optimization problems, providing a robust theoretical framework.

### Disadvantages

1. **High Memory Usage**:\
   UCS requires maintaining a priority queue and storing visited nodes, which can lead to significant memory consumption, especially in large graphs.
2. **Slow for Large Graphs**:\
   In cases of very large graphs, UCS can become computationally expensive due to its exhaustive exploration of all possible paths.
3. **Sensitive to Edge Costs**:\
   If edge costs are non-positive or not well-defined, UCS may not function correctly or may fail to terminate.
4. **Inapplicability to Unknown Goals**:\
   UCS requires a well-defined goal node, making it unsuitable for problems where the goal is not known in advance.

Uniform Cost Search is a cornerstone algorithm in artificial intelligence for optimal pathfinding. Despite its limitations, its ability to ensure optimality and handle weighted edges makes it indispensable for solving complex real-world problems.

### Example

In the search tree below, the bidirectional search algorithm is applied. This approach splits the graph or tree into two sub-graphs. One search begins from the starting node (node **A**) and proceeds forward, while the other starts from the goal node (node **O**) and moves backward.

The algorithm concludes at node **H**, where the two searches intersect.

<div align="left"><figure><img src="../../../../.gitbook/assets/image (151).png" alt="" width="563"><figcaption></figcaption></figure></div>

**Completeness**:&#x20;

Bidirectional search is complete when BFS is used for both forward and backward searches.

**Time Complexity**:&#x20;

The time complexity of bidirectional search using BFS is $$O(b^\frac{d}{2})$$ .

**Space Complexity**:&#x20;

The space complexity of bidirectional search is $$O(b^\frac{d}{2})$$.

**Optimality**:&#x20;

Bidirectional search is optimal.
