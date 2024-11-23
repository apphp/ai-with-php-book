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
