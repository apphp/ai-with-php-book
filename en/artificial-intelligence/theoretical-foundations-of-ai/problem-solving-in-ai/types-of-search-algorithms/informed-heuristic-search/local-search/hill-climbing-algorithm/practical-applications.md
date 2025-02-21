# Practical Applications

Let's review practical usage of  Hill Climbing Search for Traveling Salesman Problem (TSP) - a classic optimization problem in computer science and operations research. It involves finding the shortest possible route for a salesman to visit a set of cities exactly once and return to the starting point.

#### Example:



#### Result:

```
Creating city graph and running all search algorithms...
Start: Philadelphia, Goal: Houston

--------------------------------------------------
Simple Hill Climbing:
--------------------------------------------------
Path sequence:
 -> Philadelphia
 -> New York
 -> Miami
 -> Houston

Path analysis:
Step 1: PHL (level 0, h=3843.5) -> NY (level 1, h=3835.7): cost: 129.6
Step 2: NY (level 1, h=3835.7) -> MI (level 2, h=3758.8): cost: 1757.9
Step 3: MI (level 2, h=3758.8) -> HO (level 3, h=2206.3): cost: 1556.8
Step 4: HO (level 3, h=2206.3)

Total path cost: 3444.3
Time taken: 0.0000 seconds
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
