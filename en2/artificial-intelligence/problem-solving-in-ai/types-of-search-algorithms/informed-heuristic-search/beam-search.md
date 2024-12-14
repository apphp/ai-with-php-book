# Beam Search

Beam Search is a heuristic search algorithm designed to balance memory consumption and search efficiency in graph-based problems. It is an optimization of the best-first search algorithm, but with a key difference: it retains only a limited number of “best” nodes, referred to as the beam width (β), at each level of the search tree. While it sacrifices completeness and optimality, Beam Search is particularly effective in domains where memory limitations or large search spaces make exhaustive search impractical.

The algorithm works by employing a breadth-first search approach, expanding nodes level by level. At each level, it evaluates all successors of the current states, sorts them by a heuristic cost function, and retains only the top β nodes. This makes it a greedy algorithm, focusing only on the most promising paths at any given time.

Common applications of Beam Search include **machine translation**, **speech recognition**, and **sequence prediction** tasks, where it is essential to manage memory while exploring a vast number of possibilities.

### Key Characteristics

1. **Beam Width:**\
   The beam width (β) determines the number of nodes retained at each level of the search. A larger β increases the chance of finding an optimal solution but consumes more memory and computation time. With a beam width of infinity, the algorithm becomes equivalent to **breadth-first search**.
2. **Heuristic Evaluation:**\
   Beam Search relies heavily on a heuristic function to rank nodes. Nodes with the lowest heuristic costs are retained as potential candidates for further expansion. The quality of the heuristic function significantly impacts the algorithm’s efficiency and effectiveness.
3. **Memory Management:**\
   By limiting the number of nodes stored at each level, Beam Search has a linear memory consumption proportional to the beam width and the maximum depth of the search tree. This makes it suitable for exploring large search spaces.
4. **Pruning:**\
   Nodes not within the top β are pruned, meaning they are discarded and no longer considered. This pruning enables the algorithm to maintain tractability but can result in missed goal states or suboptimal solutions.
5. **Variants:**\
   Beam Search has several variants, including:
   * **Stochastic Beam Search**: Introduces randomness in selecting the next β states to avoid local maxima.
   * **Beam Stack Search**: Combines depth-first search with Beam Search to ensure completeness.
   * **Flexible Beam Search**: Adjusts beam width dynamically based on the problem’s complexity

### Advantages

1. **Memory Efficiency**:\
   Beam Search only stores a limited number of nodes (β) at each level, making it memory-efficient compared to exhaustive search algorithms like breadth-first search or A\*.
2. **Scalability to Large Search Spaces**:\
   By focusing on a subset of the most promising nodes, Beam Search can explore very large search spaces without being overwhelmed by memory or computational constraints.
3. **Faster Search Times**:\
   The pruning mechanism enables the algorithm to focus only on the most promising paths, reducing the time spent exploring less optimal options.
4. **Effective in Practice**:\
   Despite its theoretical limitations, Beam Search performs well in real-world applications, including:
   * Speech recognition (e.g., Harpy Speech Recognition System, CMU 1976)
   * Machine translation
   * Sequence prediction in machine learning tasks

### Disadvantages

1. **Incompleteness:**\
   Beam Search is not guaranteed to find a solution if one exists, as the goal state may be pruned during the search process.
2. **Suboptimal Solutions:**\
   The algorithm does not guarantee that the solution found is the best possible. This is especially true for small beam widths or inaccurate heuristic functions.
3. **Dependency on Heuristics:**\
   The success of Beam Search heavily depends on the quality of the heuristic function. Poor heuristics can lead the algorithm astray, resulting in wasted computation and suboptimal solutions.
4. **Beam Width Sensitivity:**\
   The choice of beam width is critical. A small beam width can result in excessive pruning, missing the goal state, while a large beam width increases memory consumption and slows down computation.
5. **Local Maxima Issues:**\
   Beam Search may become stuck in local maxima due to its greedy nature, particularly in complex search spaces.

### Example





#### Time Complexity

The time complexity of Beam Search depends on the beam width (β) and the depth of the search tree (m):&#x20;

$$\text{Time Complexity} = O(\beta \cdot m)$$

Where:

* **β**: The beam width, i.e., the number of nodes expanded at each level.
* **m**: The maximum depth of the search tree.

In the worst case, Beam Search evaluates β nodes at each depth level, making it efficient for problems where β is small.

#### Space Complexity

The space complexity of Beam Search is linear, determined by the beam width and the depth of the tree: &#x20;

$$\text{Space Complexity} = O(\beta \cdot m)$$

Since Beam Search stores only β nodes at each level, its memory consumption is significantly lower than exhaustive algorithms like breadth-first search, making it ideal for memory-constrained environments.

#### Completeness

Beam Search is **not complete**, meaning it does not guarantee finding a solution even if one exists. This is because the algorithm prunes nodes that are not among the top β at each level, which could include paths leading to a solution.

#### Optimality

Beam Search is **not optimal**, as it does not guarantee finding the best solution. The algorithm focuses on heuristic evaluations to determine promising nodes but can miss the globally optimal path due to pruning and the greedy nature of the search

To improve completeness and optimality:

* A larger beam width can reduce the chances of pruning critical nodes, though this increases computational and memory requirements.
* A better heuristic function can guide the search toward more promising solutions.

### Conclusion

Beam Search is a powerful and efficient algorithm for tackling problems where memory constraints or large search spaces make exhaustive search impractical. By retaining only a limited number of the most promising nodes at each level, it strikes a balance between efficiency and tractability. However, its reliance on heuristics, sensitivity to beam width, and lack of completeness or optimality make it less suitable for problems requiring guaranteed solutions. Despite these drawbacks, Beam Search remains a popular choice in domains where practical efficiency often outweighs theoretical guarantees.
