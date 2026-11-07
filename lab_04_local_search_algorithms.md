# Lab 04: Local Search Optimization — N-Queens Problem

## Exercise 1: Empirical Study on Scaling Board Sizes ($N$)

Performance evaluation of standard steepest-ascent Hill-Climbing across increasing board dimensions ($N = 4, 8, 12, 16$).

### Benchmark Results

| Board Dimension ($N$) | Trial Run | Residual Conflicts | Step Iterations | Success Status | 
| ----- | ----- | ----- | ----- | ----- | 
| **4** | 1 | 1 | 3 | False | 
| **4** | 2 | 0 | 4 | True | 
| **4** | 3 | 0 | 3 | True | 
| **8** | 1 | 2 | 5 | False | 
| **8** | 2 | 1 | 6 | False | 
| **8** | 3 | 0 | 7 | True | 
| **12** | 1 | 2 | 8 | False | 
| **12** | 2 | 1 | 6 | False | 
| **12** | 3 | 3 | 5 | False | 
| **16** | 1 | 3 | 9 | False | 
| **16** | 2 | 2 | 8 | False | 
| **16** | 3 | 1 | 11 | False | 

### Performance Observations

* **Local Optima Trap:** As board size $N$ expands, standard steepest-ascent hill climbing frequently gets trapped in local minima or shoulder plateaus where no immediate single-queen move reduces the heuristic conflict count.
* **Success Rate Decay:** The probability of randomly generating an initial state that leads directly to a global minimum ($0$ conflicts) decreases rapidly as $N$ increases.

---

## Exercise 2: Comparative Analysis — Large Board Scale ($N = 25$)

Comparative performance between **Standard Hill-Climbing** and **Random-Restart Hill-Climbing** on a high-dimensional state space ($N = 25$).

### Comparative Performance Table

| Performance Criterion | Steepest-Ascent Hill-Climbing | Random-Restart Hill-Climbing | 
| ----- | :---: | :---: | 
| **Residual Conflict Count** | 2 | 0 | 
| **Steps per Search Attempt** | 11 | 9.4 (Avg) | 
| **Total Restarts Required** | N/A | 38 | 
| **Optimal Solution Reached?** | No | **Yes** | 

### Key Findings & Discussion

1. **Robustness of Random Restarts:** While basic hill-climbing easily gets stuck in non-goal local minima (residual conflicts $> 0$), random-restart guarantees finding a global solution given sufficient attempts by resampling the initial configuration space.
2. **Trade-off:** Random-restart overcomes local maxima and plateaus at the expense of higher overall computational time, requiring multiple search iterations until a viable search trajectory is sampled.