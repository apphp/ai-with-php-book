# Depth-First Search (DFS)

DFS explores as deeply as possible along each branch before backtracking. It uses less memory than BFS but may not find the optimal solution.

* **Time Complexity**: $$O(b^m)$$, where  is the maximum depth of the tree.
* **Space Complexity**: $$O(b \times m)$$.
* **Use Case**: Puzzle-solving where solutions are deep in the tree.

#### 4. Depth-Limited Search

DFS with a depth limit to prevent infinite exploration. It is incomplete if the goal lies beyond the depth limit.

* **Time Complexity**: Same as DFS.
* **Space Complexity**: Same as DFS.
* **Use Case**: Solving problems with known depth constraints.
