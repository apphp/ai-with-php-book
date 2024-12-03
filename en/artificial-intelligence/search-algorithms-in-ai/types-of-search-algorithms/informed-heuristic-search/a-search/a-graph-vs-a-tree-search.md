# A\* Graph vs A\* Tree Search

### Difference Between A\* Graph Search and A\* Tree Search

<table><thead><tr><th width="196">Feature	</th><th width="277">A* Tree Search</th><th>A* Graph Search</th></tr></thead><tbody><tr><td>Re-exploration</td><td>Nodes can be re-expanded multiple times if encountered through different paths.</td><td>Nodes are not re-expanded; already explored nodes are tracked and skipped.</td></tr><tr><td>Tracking Explored Nodes</td><td>Does not maintain a record of explored nodes, leading to potential redundancy.</td><td>Maintains a closed list (or equivalent structure) to track explored nodes and avoid duplication.</td></tr><tr><td>Efficiency</td><td>Can be inefficient due to redundant exploration of the same nodes.</td><td>More efficient as it avoids re-exploring nodes.</td></tr><tr><td>Memory Usage</td><td>Requires less memory because it does not track explored nodes.</td><td>Requires more memory to maintain the list of explored nodes.</td></tr><tr><td>Optimality</td><td>Optimal only if no node is revisited, which is not guaranteed in complex graphs.</td><td>Guarantees optimality if the heuristic is consistent.</td></tr><tr><td>Heuristic Requirements</td><td>The heuristic needs to be admissible (underestimates the true cost).</td><td>The heuristic must be consistent (ensuring h(A) - h(B) \leq g(A \to B) ) for guaranteed optimality.</td></tr><tr><td>Suitable Scenarios</td><td>Suitable for simpler search spaces or scenarios where memory is constrained.</td><td>Preferred for complex graphs with many interconnected nodes.</td></tr></tbody></table>

### Summary

A\* Graph Search improves on A\* Tree Search by eliminating redundancy in node expansion through the use of a closed list. While it may require more memory, it significantly enhances efficiency and guarantees optimality with a consistent heuristic, making it more suitable for solving complex problems involving large graphs.

