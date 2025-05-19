# DS425 – Pathfinding on the Moon

## Authors
- De Neve Senne
- Laceur Niels
- Van der Veken Pieter
- Van Sande Wouter

---

## Introduction
To solve this somewhat fictional illustration, the matter could be seen as a combination of two independent subcases:
1. Finding the best possible route for the rover to travel from one lunar lander to another (local pathfinding).
2. Calculating the best overall order in which to visit the landers (global optimization).

---

## 1. Used Methods

### 1.1 A* Algorithm

The A* algorithm is used to determine the shortest path between two points. It evaluates paths using a cost function:


- `g`: Cost from the start node to the current node.
- `h`: Heuristic estimate of the cost from the current node to the goal.

The heuristic must be admissible, such as Euclidean or Manhattan distance. The cost can be customized depending on terrain features like elevation or soil type.

---

### 1.2 Travelling Salesman Problem (TSP)

The TSP aims to find the shortest route that visits a set of nodes exactly once and returns to the starting point. It is NP-hard, making exact solutions infeasible for large instances. Approximation algorithms are therefore used.

#### 1.2.1 Christofides Algorithm

This approximation algorithm follows these steps:

1. Compute the Minimum Spanning Tree (MST).
2. Find nodes of odd degree in the MST and perform minimum-weight perfect matching.
3. Combine the MST and the matching to form an Eulerian graph.
4. Find an Eulerian tour.
5. Convert the Eulerian tour to a Hamiltonian circuit by skipping repeated nodes.

The Christofides algorithm guarantees a solution within 1.5 times the optimal solution.

---

### 1.3 Kruskal Algorithm

Used to compute the MST. It is a greedy algorithm that adds the lowest-weight edge that doesn't create a cycle. Initially, this can form multiple trees, which are eventually merged.

---

### 1.4 Simulated Annealing

A probabilistic optimization technique inspired by the annealing process in metallurgy. It begins with an initial solution and a high "temperature". At each step:

- A new solution is generated.
- If it's better, it is accepted.
- If it's worse, it may still be accepted with a probability:

P(accept) = e^(−ΔE / T)

Where `ΔE` is the increase in cost and `T` is the current temperature.

The temperature is gradually decreased, reducing the chance of accepting worse solutions.

---

## 2. Implementation & Discussion

### 2.1 A* Algorithm

Implemented to calculate optimal paths between every pair of checkpoints on a terrain that varies by soil type and elevation. Terrain types included rock, gravel, and dust, with color-coding for visual reference.

This output serves as the basis for cost calculations between landers in the TSP.

---

### 2.2 Travelling Salesman Problem

Steps followed:
- A complete graph was created with all checkpoint connections.
- Kruskal’s algorithm was applied to generate the MST.
- Odd-degree nodes were matched with minimum-cost edges.
- An Eulerian tour was computed.
- Repeated nodes were removed to form a TSP tour.

The resulting TSP tour had a total cost of **3478**.

---

### 2.3 Solution Optimization using Local Search

Simulated Annealing was applied to the result from the Christofides algorithm for further improvement. 

Results:
- Before annealing: Cost = 1375
- After annealing: Cost = 1307

A reduction of approximately 1.052%.

---

## 3. Conclusion

- A* was used to find optimal paths between nodes, considering terrain and elevation.
- Christofides algorithm (with Kruskal for MST) was applied to solve the TSP.
- Simulated Annealing improved the initial TSP solution further.

This multi-step hybrid approach effectively solved the pathfinding challenge in an efficient and modular way.

---

## References

1. [GeeksforGeeks: A* Search Algorithm](https://www.geeksforgeeks.org/a-search-algorithm/)
2. [GeeksforGeeks: Simulated Annealing](https://www.geeksforgeeks.org/what-is-simulated-annealing/)
3. Johnson, D. S., & McGeoch, L. A. (1997). *The Traveling Salesman Problem: A Case Study in Local Optimization*. In: Local Search in Combinatorial Optimization.
4. [Kolaru. SimulatedAnnealing.jl](https://juliapackages.com/p/simulatedannealing)
5. Kumar, D. A., & Rangan, C. P. (2000). Approximation Algorithms for the TSP with Range Condition. *RAIRO - Theoretical Informatics and Applications*.
6. [Roy Matthew et al. Traveling Salesman Algorithms](https://cse442-17f.github.io/Traveling-Salesman-Algorithms/)
7. [Reducible: The Traveling Salesman Problem](https://www.youtube.com/watch?v=GiDsjIBOVoA)
