# Lab 02: Uninformed Search Strategies (BFS & DFS on Grid Environments)

## Section 1: Comparative Evaluation of BFS and DFS

Evaluating Breadth-First Search (BFS) and Depth-First Search (DFS) on a standard 6x6 grid maze instance.

| Search Strategy | Calculated Path Length | Max Frontier Memory Load |
| :--- | :--- | :--- |
| **Breadth-First Search (BFS)** | 10 | 4 |
| **Depth-First Search (DFS)** | 14 | 5 |

### Analysis Questions & Discussion

1. **Which strategy identified the shorter trajectory?**  
   Breadth-First Search (BFS) successfully determined the shorter route, completing the traversal in 10 steps compared to DFS's 14 steps.

2. **Which strategy expanded the larger maximum frontier queue/stack?**  
   Depth-First Search (DFS) maintained a larger maximum frontier size during this execution (5 nodes versus 4 nodes in BFS).

3. **Why do the two algorithms generate non-identical routes on an identical grid layout?**  
   The variance stems from their fundamental node expansion policies. BFS operates in an expanding layer-by-layer sequence (FIFO queue), guaranteeing uniform radial expansion. In contrast, DFS pushes as deeply as possible down a single exploration path (LIFO stack) before backtracking upon hitting dead ends or obstacles.

4. **Is DFS guaranteed to locate the optimal (shortest) route? Explain.**  
   No. DFS explores deeply down the first available path matching its directional priorities. It terminates immediately upon reaching the target node, regardless of whether a shorter alternate path existed unexplored in another branch.

5. **Why does BFS consistently guarantee the shortest path in unweighted environments?**  
   Because every edge transition carries a uniform step cost of 1, expanding nodes strictly in order of non-decreasing distance ensures that the first time the target node is popped from the frontier, it has been reached via the fewest possible total steps.

---

## Section 2: Sensitivity Analysis on Neighbor Traversal Order

The trajectory traced by DFS relies heavily on the sequence in which movement vectors (neighbors) are evaluated.

### Setup A: Default Direction Bias
**Movement Priority Sequence:**
1. Down $(1, 0)$
2. Right $(0, 1)$
3. Up $(-1, 0)$
4. Left $(0, -1)$

**Execution Metrics:**
* **DFS Traced Trajectory:** `[(0, 0), (1, 0), (2, 0), (3, 0), (3, 1), (4, 1), (5, 1), (5, 2), (5, 3), (5, 4), (5, 5)]`
* **Path Length:** 10 steps
* **Max Frontier Size:** 4

---

### Setup B: Modified Direction Bias
**New Priority Sequence:**
1. Right $(0, 1)$
2. Down $(1, 0)$
3. Left $(0, -1)$
4. Up $(-1, 0)$

**Execution Metrics:**
* **DFS Traced Trajectory (New Priority):** `[(0, 0), (0, 1), (0, 2), (1, 2), (1, 3), (1, 4), (2, 4), (3, 4), (4, 4), (4, 5), (5, 5)]`
* **Path Length:** 10 steps
* **Max Frontier Size:** 5

### Impact Analysis of Direction Reordering

Changing direction priority alters the branch that DFS commits to first, yielding a totally different cell trajectory. 

While BFS may also alter its specific intermediate path sequence under different neighbor orderings (when multiple equal-length shortest paths exist), its **total path length will always remain optimal** in unit-cost state spaces.

---

## Section 3: Variable Start/Goal Coordinate Experiments

Evaluating performance variation across three distinct coordinate configurations:

### Benchmark Trials

| Trial | Start Coordinate | Goal Coordinate | BFS Path Length | BFS Max Frontier | DFS Path Length | DFS Max Frontier |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Trial 1** | $(0, 0)$ | $(3, 3)$ | 6 | 4 | 12 | 4 |
| **Trial 2** | $(1, 1)$ | $(5, 2)$ | 5 | 3 | 9 | 3 |
| **Trial 3** | $(0, 5)$ | $(5, 0)$ | 10 | 5 | 10 | 6 |

### Conceptual Review Questions

6. **Does the path length for BFS vary across identical runs with static start/goal parameters? Does DFS vary?**  
   BFS yields a deterministic path length every time for fixed start/goal coordinates. DFS is also deterministic for a fixed neighbor order, but its path length fluctuates dramatically if the neighbor evaluation order is modified between runs.

7. **How does spatial separation between start and goal impact maximum frontier growth?**  
   Increasing distance generally increases memory consumption for both strategies because more open branches must be tracked simultaneously. However, frontier size is primarily constrained by maze branching factors and wall configurations rather than absolute distance alone.

8. **Under what maze structural conditions do BFS and DFS yield identical paths?**  
   BFS and DFS generate identical paths when the state space contains a single linear path to the goal (no alternative branch points), or when DFS's primary direction preference happens to directly align with the optimal shortest path without forcing any dead-end backtracking.

9. **What is the operational behavior of both algorithms when encountering an unreachable goal state?**  
   If the goal is fully enclosed by obstacles, both algorithms exhaustively visit every accessible node in the connected graph component. Upon draining the frontier without locating the target, both functions return `None` (or `False`) along with the peak frontier count logged during the complete search space traversal.