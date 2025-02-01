# Global Search

Uninformed search algorithms do not have any domain-specific knowledge; they rely only on the problem definition (e.g., the initial state, goal state, and available actions). Below the global uninformed algorithms **(exploring the entire space):**

* **Breadth-First Search (BFS)**: Explores all nodes at the current depth before moving deeper.
* **Depth-First Search (DFS)**: Explores as far as possible down one path before backtracking.
* **Uniform-Cost Search (UCS)**: Expands the least-cost node first, ensuring optimality if costs are uniform or positive.
* **Iterative Deepening Search (IDS)**: Combines DFS and BFS by performing depth-limited searches with increasing limits.
* **Bidirectional Search (BDS)**: Explores the search space simultaneously from the initial state and the goal state, meeting in the middle.
