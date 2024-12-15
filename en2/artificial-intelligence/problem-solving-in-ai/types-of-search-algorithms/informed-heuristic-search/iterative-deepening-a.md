# Iterative Deepening A\*

The Iterative Deepening A\* Algorithm (IDA\*) is a heuristic search method that combines the memory efficiency of Depth-First Search (DFS) with the optimal path-finding capabilities of the A\* algorithm. It is specifically designed to handle large search spaces while maintaining optimality and completeness. By limiting memory usage, IDA\* enables effective exploration of complex networks or trees to find the shortest path from the start state to the goal state.

IDA\* leverages a cost threshold to incrementally explore deeper levels of the search space. Each iteration expands nodes that remain within the current threshold, and this value is updated progressively. This chapter delves into IDA\*’s key features, advantages, disadvantages, and practical significance.

### Key Characteristics

1. **Depth-First Search and Cost Threshold**:\
   IDA\* uses depth-first search to traverse nodes while adhering to a dynamically adjusted cost threshold. The initial threshold is determined by the heuristic estimate of the path cost from the start to the goal node. As nodes are expanded, the algorithm tracks costs exceeding this threshold and adjusts it to the minimum value of those nodes for the next iteration.
2. **Memory Efficiency**:\
   Unlike A\*, which retains the entire search tree in memory, IDA\* only maintains the current path and its cost. This significant reduction in memory usage makes IDA\* suitable for problems with large or infinite state spaces.
3. **Optimality and Completeness**:\
   IDA\* guarantees finding the optimal solution if it exists, provided the heuristic function is admissible (never overestimates the cost) and consistent (satisfies the triangle inequality). The algorithm is also complete, meaning it explores all possibilities to ensure no solution is missed.

### Advantages

1. **Memory** **Efficiency**:\
   IDA\* requires memory only for the current path, making it feasible for problems with extensive search spaces where A\* would fail due to memory limitations.
2. **Optimality**:\
   The algorithm ensures an optimal solution as long as the heuristic is admissible.
3. **Incremental Search**:\
   Each iteration builds upon the previous one, allowing for better control over the search depth and making the algorithm interruptible and resumable without loss of progress.
4. **Flexibility with Heuristics**:\
   IDA\* supports various heuristic functions, enabling adaptation to diverse problem domains.
5. **Wide Applicability**:\
   The algorithm is effective in real-world scenarios such as pathfinding, scheduling, and game AI where optimal solutions are required under memory constraints.

### Disadvantages

1. **Recomputations**:\
   IDA\* revisits and re-expands nodes across iterations, leading to redundant calculations and potentially higher time complexity compared to A\*.
2. **Performance Dependence on Heuristics**:\
   The algorithm’s efficiency heavily relies on the quality of the heuristic function. Poor heuristics can lead to slow convergence.
3. **Limited to Problems with Defined Goals**:\
   IDA\* is not suitable for problems lacking clearly defined or identifiable goal states.
4. **Potential for Local Minima**:\
   IDA\* can get stuck in local minima if the heuristic function is not consistent, affecting its ability to explore alternative paths.
5. **Memory Use in Certain Scenarios**:\
   Although memory-efficient, some specific search problems might still require significant memory due to the depth of the state space.

### Example

<div align="left"><figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

To understand how the Iterative Deepening A\* (IDA\*) algorithm works, let’s consider a simple graph. In this graph, the numbers in parentheses represent the cost of traveling between nodes.

We aim to find the optimal path from node A to node F using the IDA\* algorithm. Here’s how the process unfolds step by step:

1. **Set the Initial Cost Limit**\
   The algorithm begins by setting an initial cost limit. For this example, we’ll use the heuristic estimate of the optimal path, which is 7 (the sum of costs along the path $$A→C→F$$ ).
2. **Start at Node A**\
   Begin the search at node A and expand its neighbors: B and C.
3.  **Evaluate Paths from A**

    1. The cost of the path is 5.
    2. The cost of the path $$A→C$$ is 10.

    &#x20;Since has a cost less than the current cost limit (7), proceed with node B.
4.  **Expand Node B**\
    From B, expand its neighbors: D and E.

    1. The cost of the path is 10.
    2. The cost of the path $$A→E$$ is 9.

    &#x20;Both paths exceed the cost limit (7), so backtrack to node A.
5. **Reevaluate Node C**\
   Reevaluate the path $$A→C$$, which has a cost of 10. Since it also exceeds the current cost limit, backtrack to node A and increase the cost limit.
6. **Update Cost Limit**\
   Adjust the cost limit to the lowest cost of any node that exceeded the previous threshold, and repeat the process. Continue this until all feasible paths within the new cost limit are explored.
7. **Find the Optimal Path**\
   Eventually, the algorithm identifies $$A→C→F$$ as the optimal path, with a total cost of 7.

#### Key Takeaways

* **Initial Exploration**: The algorithm starts with the most promising paths based on the heuristic estimate.
* **Incremental Adjustment**: By gradually increasing the cost limit, IDA\* ensures no potential solutions are overlooked.
* **Backtracking**: If all paths exceed the current threshold, the algorithm backtracks, updates the cost limit, and repeats the search.

The IDA\* algorithm systematically narrows down the search until it identifies the shortest path. Its iterative nature ensures completeness and optimality while keeping memory usage efficient.

### Complexity, **Completeness** and Optimality

#### Time Complexity

The time complexity of IDA\* depends on the branching factor (), the depth of the optimal solution (), and the heuristic function’s quality.

* **Best Case**: When the heuristic function provides excellent guidance, the algorithm reduces unnecessary expansions, making it efficient.
* **Worst Case**: Similar to Depth-First Search (DFS), IDA\* may revisit nodes in successive iterations, leading to repeated computations. The time complexity is approximately $$O(b^d)$$, where $$b$$ is the branching factor, and $$d$$ is the depth of the solution.
* **Effective Branching Factor**: IDA\* performs better with an admissible and consistent heuristic, reducing unnecessary expansions and improving overall efficiency.

#### Space Complexity

IDA\* is highly space-efficient, requiring memory proportional to the depth of the search tree. Unlike A\*, which stores all visited nodes, IDA\* only keeps track of the current path and associated costs.

* **Space Complexity**: $$O(d)$$, where is the depth of the search tree. This makes it feasible for large or infinite search spaces.

#### Completeness

IDA\* is complete, meaning it will always find a solution if one exists. It systematically explores all possible paths by progressively increasing the cost threshold, ensuring no solution is missed. However, completeness assumes that the branching factor is finite and there is a defined goal state.

#### Optimality

IDA\* is optimal, provided the heuristic function is:

1. **Admissible**: It never overestimates the actual cost to reach the goal $$h(n) \leq h^*(n)$$.
2. **Consistent**: It satisfies the triangle inequality $$h(n) \leq c(n, m) + h(m)$$, where $$c(n, m)$$ is the cost to move from node $$n$$ to $$m$$.

Under these conditions, IDA\* guarantees finding the least-cost path to the goal.

### Conclusion

In conclusion, IDA\* is a memory-efficient and optimal algorithm that combines the best of Depth-First Search and A\*. However, its time complexity can be high due to node recompilations in successive iterations, making the choice of heuristic crucial for performance.
