# Greedy Search

Greedy search is an informed search algorithm that aims to expand the node closest to the goal, as estimated by a heuristic function . It takes a direct and straightforward approach, always choosing the path that seems most promising based on the heuristic value. The method is inspired by human intuition — choosing the option that appears best at each step without considering the overall problem structure. While simple and often efficient, greedy search is not guaranteed to find the optimal solution.

### Key Characteristics

1. **Heuristic-Driven Approach**:\
   Greedy search relies heavily on a heuristic function, , which estimates the cost or distance from a given node to the goal node. The algorithm prioritizes nodes with lower heuristic values, as they are considered “closer” to the goal.\
   Heuristic Function: $$h(x) =$$ Estimate of distance of node $$x$$ from the goal node.\
   Lower the value of $$h(x)$$, closer is the node from the goal.
2. **Node Expansion Strategy**:\
   Nodes are expanded based on their heuristic value. The node with the lowest is expanded first, reflecting the algorithm’s pursuit of the shortest perceived path to the goal.
3. **No Backtracking**:\
   Greedy search does not account for the total path cost from the start node to the current node; it only considers the heuristic to the goal. This makes it efficient but prone to getting stuck in local optima or dead ends.
4. **Algorithm Behavior**:\
   •  Starts at the initial node.\
   •  Evaluates and selects the node with the smallest from the frontier (open list).\
   • Repeats the process until the goal node is reached or no further nodes are available.

### Advantages

1. **Efficiency in Simple Problems**:\
   Greedy search can quickly find a solution for problems where the heuristic accurately reflects the path to the goal. Its simplicity and speed make it ideal for problems with well-behaved heuristics.
2. **Ease of Implementation**:\
   The algorithm is straightforward to code and understand, making it a popular choice for certain types of searches and optimization problems.
3. **Focus on Goal**:\
   By directly expanding nodes closest to the goal, greedy search minimizes unnecessary exploration, focusing on the most promising routes.

### Disadvantages

1. **Lack of Optimality:**\
   Greedy search does not guarantee the optimal solution. It may settle for a suboptimal solution if the heuristic misleads it toward a local minimum or a longer path.
2. **Incomplete Search:**\
   If the heuristic function is poorly designed or the search space is too complex, the algorithm might fail to find a solution even if one exists.
3. **Sensitivity to Heuristic:**\
   The success of greedy search is highly dependent on the quality of the heuristic. An inaccurate heuristic can lead to inefficient or incorrect results.
4. **No Cost Awareness:**\
   Greedy search does not account for the actual cost incurred from the start node to the current node, which can result in selecting paths that seem promising initially but are more expensive overall.

In conclusion, greedy search is a powerful algorithm when used in the right contexts. Its reliance on heuristic-driven expansion allows for rapid solutions but can also limit its effectiveness in scenarios with complex or deceptive search spaces. Understanding its strengths and limitations is key to applying it effectively in artificial intelligence and other computational problems.

### **Example**

Find the path from S to G using greedy search. The heuristic values $$h$$ of each node below the name of the node.

Starting from node S , we can move to either A (h = 9) or D (h = 5). Since D has the lower heuristic value, we select it. From D , we can proceed to either B (h = 4) or E (h = 3). Again, we choose E because it has the smaller heuristic value. Finally, from E , we move to G (h = 0), which is the goal node. The complete traversal is illustrated in the search tree below, highlighted in dashed red.

The sequence of nodes visited during traversal will be:&#x20;

$$S→D→E→G$$

<div align="left"><figure><img src="../../../../.gitbook/assets/image (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

#### Time Complexity:

The time complexity depends on the branching factor $$b$$ and the maximum depth $$m$$ of the search tree. In the worst case, the complexity is , as it may need to explore all nodes.

#### Space Complexity:

The space complexity is also $$O(b^m)$$, as it stores all generated nodes in memory, including the frontier.

#### Completeness:

Greedy search is incomplete. It may fail to find a solution if the search space is infinite or if the heuristic leads the search down a non-terminating path.

#### Optimality:

Greedy search is not optimal, as it does not consider the total cost of the path and may return a suboptimal solution.

In conclusion, greedy search is a powerful algorithm when used in the right contexts. Its reliance on heuristic-driven expansion allows for rapid solutions but can also limit its effectiveness in scenarios with complex or deceptive search spaces.
