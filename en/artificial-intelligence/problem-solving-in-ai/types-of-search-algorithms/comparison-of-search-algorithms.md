# Comparison of Search Algorithms

### Comparison of Uninformed and Informed Search Algorithms  <a href="#comparison-of-uninformed-and-informed-search-algorithms" id="comparison-of-uninformed-and-informed-search-algorithms"></a>

| Feature                | Uninformed Search     | Informed Search            |
| ---------------------- | --------------------- | -------------------------- |
| **Knowledge Required** | None                  | Domain-specific heuristics |
| **Efficiency**         | Lower                 | Higher                     |
| **Optimality**         | Sometimes (e.g., UCS) | Often (e.g., A\*)          |
| **Applicability**      | General problems      | Specific, complex problems |
| **Examples**           | BFS, DFS, UCS         | Greedy, A\*, Hill Climbing |

### **Comparison of Global and Local Search Algorithms**

<table><thead><tr><th width="166">Feature</th><th width="321">Global Search</th><th>Local Search</th></tr></thead><tbody><tr><td><strong>Scope</strong></td><td>Explores the entire search space</td><td>Explores a localized region</td></tr><tr><td><strong>Efficiency</strong></td><td>Often slower due to exhaustive exploration</td><td>Faster due to focused search</td></tr><tr><td><strong>Optimality</strong></td><td>More likely to find the global optimum</td><td>Can get stuck in local optima</td></tr><tr><td><strong>Applicability</strong></td><td>Large, complex problems requiring global solutions</td><td>Problems where a good enough solution is acceptable</td></tr><tr><td><strong>Examples</strong></td><td>Genetic, Simulated Annealing</td><td>Hill Climbing, Tabu Search</td></tr></tbody></table>

### Advanced Search Techniques <a href="#advanced-search-techniques" id="advanced-search-techniques"></a>

While uninformed and informed searches form the foundation of search algorithms, advanced techniques extend their capabilities:

1. **Metaheuristic Algorithms**: Techniques like Genetic Algorithms, Simulated Annealing, and Particle Swarm Optimization optimize complex problems by combining heuristic principles with stochastic methods.
2. **Adversarial Search**: Applied in competitive environments, such as games, where multiple agents with conflicting goals interact (e.g., the Minimax algorithm).
3. **Constraint Satisfaction Problems (CSPs)**: Focused on finding solutions that meet specific constraints, often using methods like backtracking and local search.
