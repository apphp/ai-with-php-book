# Simulated Annealing Search

Simulated Annealing (SA) is an optimization algorithm inspired by the annealing process in metallurgy, where materials are heated and slowly cooled to reach a stable state with minimal energy. It is used to find approximate solutions to optimization problems by iteratively exploring potential solutions and occasionally accepting worse solutions to escape local optima.

### Key Characteristics:

* **Time Complexity**: $$O(n)$$, where nn is the number of iterations before the temperature reaches the stopping criterion.
* **Space Complexity**: $$O(1)$$, as it only needs to store the current state, temperature, and energy value.

### Advantages:

* Can escape local optima by accepting worse solutions with a probability based on temperature.
* Does not require gradient information, making it suitable for discrete and continuous problems.
* Effective for complex optimization problems with large search spaces.

### Disadvantages:

* Convergence can be slow, especially if the cooling schedule is not well-tuned.
* May not always find the global optimum.
* Performance depends on parameter selection (initial temperature, cooling rate, and stopping condition).

### Use Cases:

* **Combinatorial Optimization** (e.g., Traveling Salesman Problem, scheduling problems).
* **Machine Learning** (e.g., hyperparameter tuning).
* **Computer Vision** (e.g., image segmentation, feature matching).
* **Game AI** (e.g., strategy optimization).

### Fundamental Concepts of Simulated Annealing

Simulated Annealing mimics the cooling process of metals. At high temperatures, particles move freely, but as the temperature decreases, movement slows, and the system settles into a stable state. This principle is applied in optimization problems where:

1. **Initial State**: Start with an initial solution and set an initial high temperature.
2. **Neighboring States**: Identify neighboring solutions by making small changes (mutations or tweaks) to the current state.
3. **Move to Neighbor**:
   * If the new solution is better, accept it.
   *   If it is worse, accept it with a probability determined by the Boltzmann probability factor:

       $$P= e^{\frac{-(\Delta E)}{T}}$$

       Where $$\Delta E$$ is the change in solution quality, and $$T$$ is the temperature.
4. **Cooling**: Reduce the temperature following a cooling schedule (e.g., geometric decay: $$T = \alpha T \ with \ 0 < \alpha < 1$$).
5. **Termination**: Repeat the process until the stopping condition (temperature threshold or iteration limit) is met.

### Implementing Simulated Annealing in PHP

Below is a PHP implementation of Simulated Annealing for solving an optimization problem (e.g., finding the minimum value of a mathematical function).

```php
<?php

class SimulatedAnnealing {
    private $temperature;
    private $coolingRate;
    private $stopTemperature;

    public function __construct($initialTemperature, $coolingRate, $stopTemperature) {
        $this->temperature = $initialTemperature;
        $this->coolingRate = $coolingRate;
        $this->stopTemperature = $stopTemperature;
    }

    private function objectiveFunction($x) {
        // Example: Finding the minimum of x^2
        return pow($x, 2);
    }

    private function getRandomNeighbor($x) {
        // Generate a small random step
        return $x + ((rand(0, 1000) / 1000) - 0.5) * 2;
    }

    private function acceptanceProbability($currentEnergy, $newEnergy) {
        if ($newEnergy < $currentEnergy) {
            return 1.0;
        }
        return exp(($currentEnergy - $newEnergy) / $this->temperature);
    }

    public function optimize($initialSolution) {
        $currentSolution = $initialSolution;
        $currentEnergy = $this->objectiveFunction($currentSolution);

        while ($this->temperature > $this->stopTemperature) {
            $newSolution = $this->getRandomNeighbor($currentSolution);
            $newEnergy = $this->objectiveFunction($newSolution);

            if ($this->acceptanceProbability($currentEnergy, $newEnergy) > mt_rand() / mt_getrandmax()) {
                $currentSolution = $newSolution;
                $currentEnergy = $newEnergy;
            }

            $this->temperature *= $this->coolingRate;
        }
        
        return $currentSolution;
    }
}

// Example usage
$sa = new SimulatedAnnealing(1000, 0.99, 0.1);
$optimalSolution = $sa->optimize(10);

echo "Optimal Solution: " . $optimalSolution . "\n";
```

### Explanation of the PHP Implementation

* **Initialization**: The algorithm starts with an initial solution and parameters for temperature, cooling rate, and stopping temperature.
* **Perturbation**: A small random step is applied to explore neighboring solutions.
* **Acceptance Probability**: If the new solution is worse, it is accepted with a probability decreasing over time.
* **Cooling Schedule**: The temperature decreases gradually to refine the solution.

### Conclusion

Simulated Annealing is a powerful optimization technique that helps escape local optima and find near-optimal solutions efficiently. Implementing it in PHP demonstrates the language's versatility in AI applications. With careful tuning of parameters like cooling rate and temperature, it can be applied to a variety of real-world problems.

In the next chapter, we will explore another AI search algorithm: Genetic Algorithms, which use principles of evolution to optimize solutions.
