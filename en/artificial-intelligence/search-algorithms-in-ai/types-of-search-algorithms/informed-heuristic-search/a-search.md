# A Search\*

2\. _A Search_\*

A\* combines the path cost (g(n)) and heuristic (h(n)) to evaluate nodes:

\


f(n) = g(n) + h(n)

\


It guarantees an optimal solution if h(n) is admissible (never overestimates the actual cost).

• Time Complexity: O(b^d), where d is the depth of the optimal solution.

• Space Complexity: O(b^d).

• Advantages: Optimal and complete with a good heuristic.

• Disadvantages: Memory-intensive for large search spaces.

• Use Case: Routing and optimization problems, such as finding the shortest path in a weighted graph.

3\. Hill Climbing

A local search algorithm that iteratively moves to a neighboring node with a better heuristic value.

• Time Complexity: O(n), where n is the number of nodes in the search space.

• Space Complexity: O(1).

• Advantages: Simple and uses minimal memory.

• Disadvantages: Prone to getting stuck in local optima.

• Use Case: Optimization tasks, such as tuning parameters in machine learning models.

4\. Beam Search

Explores only a fixed number of the most promising nodes at each depth, as determined by a heuristic.

• Time Complexity: Proportional to the beam width and depth of the search.

• Space Complexity: Significantly less than BFS or A\*.

• Advantages: Memory-efficient for large problem spaces.

• Disadvantages: May miss the optimal solution if the beam width is too narrow.

• Use Case: Natural language processing tasks, such as speech recognition or translation.

5\. Iterative Deepening A\*

Combines the memory efficiency of IDDFS with the optimality of A\*. It performs a series of depth-limited A\* searches.

• Time Complexity: Similar to A\*.

• Space Complexity: Reduced compared to standard A\*.

• Advantages: Balances memory use and optimality.

• Disadvantages: May involve redundant calculations.

• Use Case: Problems requiring optimal solutions in constrained memory environments
