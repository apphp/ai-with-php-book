# Beam Search

Explores only a fixed number of the most promising nodes at each depth, as determined by a heuristic.

• Time Complexity: Proportional to the beam width and depth of the search.

• Space Complexity: Significantly less than BFS or A\*.

• Advantages: Memory-efficient for large problem spaces.

• Disadvantages: May miss the optimal solution if the beam width is too narrow.

• Use Case: Natural language processing tasks, such as speech recognition or translation.
