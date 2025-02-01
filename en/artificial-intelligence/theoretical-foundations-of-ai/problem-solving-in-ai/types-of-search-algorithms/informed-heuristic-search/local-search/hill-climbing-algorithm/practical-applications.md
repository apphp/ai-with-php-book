# Practical Applications

### Practical Applications

#### Example: Traveling Salesman Problem

Here's how to apply hill climbing to the Traveling Salesman Problem:

```php
class TSPHillClimbing {
    private $distances;
    
    public function __construct(array $distances) {
        $this->distances = $distances;
    }
    
    private function calculateTourLength(array $tour) {
        $length = 0;
        $cities = count($tour);
        
        for ($i = 0; $i < $cities; $i++) {
            $from = $tour[$i];
            $to = $tour[($i + 1) % $cities];
            $length += $this->distances[$from][$to];
        }
        
        return $length;
    }
    
    private function generateNeighbors(array $tour) {
        $neighbors = [];
        $cities = count($tour);
        
        // Generate neighbors by swapping pairs of cities
        for ($i = 0; $i < $cities - 1; $i++) {
            for ($j = $i + 1; $j < $cities; $j++) {
                $neighbor = $tour;
                $temp = $neighbor[$i];
                $neighbor[$i] = $neighbor[$j];
                $neighbor[$j] = $temp;
                $neighbors[] = $neighbor;
            }
        }
        
        return $neighbors;
    }
    
    public function solve($initialTour) {
        return $this->hillClimbingWithRandomRestarts($initialTour);
    }
}
```

###
