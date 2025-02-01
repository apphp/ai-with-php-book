# Global Search

Informed search algorithms use heuristics or domain-specific knowledge to guide the search, making them more efficient. Below the global informed algorithms **(exploring the entire space with heuristics):**

* **Greedy Best-First Search**: Expands nodes based on the heuristic (`h(n)`), aiming to quickly find a solution.
* **A\***: Uses a combination of the cost to reach a node and a heuristic estimating the cost to the goal (`f(n) = g(n) + h(n)`).
* **Iterative Deepening A\***: Heuristic search method that combines the memory efficiency of Depth-First Search (DFS) with the optimal path-finding capabilities of the A\* algorithm.
