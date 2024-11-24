# Breadth-First Search (BFS)

Breadth-First Search is a widely used search strategy for traversing trees or graphs. It explores nodes level by level, expanding all successor nodes at the current depth before moving on to the next layer. This systematic breadthwise exploration is what gives BFS its name.

The BFS algorithm starts at the root node and explores all its immediate child nodes before advancing to their successors. It is an example of a general-graph search algorithm and is implemented using a **FIFO (First-In-First-Out)** queue data structure.

### **Key Characteristics**

1. **Level-by-Level Exploration**:\
   BFS explores the graph or tree level by level, starting from the root node and visiting all its neighbors before moving to the next level.
2. **Queue-Based Mechanism**:\
   BFS uses a queue data structure to manage the nodes to be visited. Nodes are added to the queue as they are discovered and removed in the order they were added, ensuring a First-In-First-Out (FIFO) traversal pattern.
3. **Shortest Path Guarantee**:\
   For unweighted graphs, BFS guarantees the shortest path (i.e., the minimum number of edges) from the source node to any other node.
4. **Visited List or Set**:\
   BFS relies on a mechanism, such as a _visited_ list or set, to track nodes that have already been explored. This avoids revisiting nodes and helps prevent infinite loops in cyclic graphs.
5. **Graph Traversal**:\
   BFS can be applied to both directed and undirected graphs, as well as weighted graphs where edge weights are uniform or ignored.
6. **Complete Search**:\
   BFS ensures that all reachable nodes are visited. This is especially useful for problems where every node or state needs to be explored.
7. **Parallel Exploration**:\
   Unlike DFS, which focuses on depth, BFS explores all possibilities at the current level before advancing, providing a breadth-oriented view of the graph or tree.

**Complexity**

* **Time Complexity**: $$O(b^d)$$, where  is the branching factor, and  is the depth of the shallowest goal.
* **Space Complexity**: $$O(b^d)$$.
* **Use Case**: Maze-solving or traversing social networks.

These characteristics make BFS an excellent choice for problems requiring exhaustive exploration or shortest-path calculations in unweighted graphs.

BFS is a fundamental algorithm known for its systematic and predictable nature, making it ideal for scenarios requiring exhaustive exploration or guaranteed minimal paths. However, its high resource requirements can pose challenges for large or deep search spaces.

### Advantages

1. **Guaranteed Solution**: BFS ensures a solution is found if one exists within the search space.
2. **Optimal Solution**: For problems with multiple solutions, BFS identifies the solution requiring the fewest steps.
3. **Shortest Path**: BFS is particularly effective for finding the shortest path to the goal, as it explores all nodes at the same depth before delving deeper.
4. **Easy to Understand**: BFS is straightforward to implement and interpret, making it easy to rank paths based on their hierarchical depth.

### Disadvantages

1. **High Memory Usage**: BFS can consume significant memory, as it must store all nodes at the current level to expand to the next level.
2. **Time-Consuming**: If the solution lies far from the root node, BFS may require considerable time to locate it.
3. **Inefficiency in Deep Trees**: BFS is inefficient for deeply nested spaces, as it thoroughly explores all nodes at each level before progressing to the next.

### Example

In the tree structure below, we illustrate the traversal process using the BFS algorithm, starting from the root node **S** and proceeding to the goal node **K** . The BFS algorithm explores the tree layer by layer, following the path indicated by the dotted arrows. The sequence of nodes visited during traversal will be:

$$S→A→B→C→D→G→H→E→F→I→K$$

<div align="left"><figure><img src="../../../../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure></div>

#### **Time Complexity:**&#x20;

The time complexity of the BFS algorithm is determined by the number of nodes traversed until the shallowest solution node is found. Let:

* $$d$$ : The depth of the shallowest solution.
* $$b$$ : The branching factor, which is the maximum number of child nodes for any node.

At depth $$d$$ , the total number of nodes traversed is:  $$T (b) = 1+b^2+b^3+...+ b^d= O (b^d)$$

#### **Space Complexity:**&#x20;

The space complexity of BFS is determined by the memory required to store the frontier, which is the set of all nodes at the deepest level d . Since BFS explores level by level, the maximum number of nodes stored in memory corresponds to the size of the last level: $$O(b^d)$$.

#### **Completeness:**&#x20;

BFS is complete, meaning that it is guaranteed to find a solution if one exists, provided the goal node is at a finite depth in the search tree.

#### **Optimality:**&#x20;

BFS is optimal if the path cost is a non-decreasing function of the depth of the node. For example, in a uniform-cost search where all step costs are equal, BFS will find the optimal solution.
