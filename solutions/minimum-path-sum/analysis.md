---
problem: "Minimum Path Sum"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.4 MB
date: 2026-07-27
---

# Analysis

### Verdict summary
This solution uses a correct dynamic programming approach by building a 2D DP table where each cell stores the minimum path sum to reach that cell. It correctly handles movement constraints (only down or right) and is accepted. However, the implementation has several inefficiencies in both memory usage and code clarity.

### Complexity
**Time Complexity:** O(m×n) - The algorithm processes each cell exactly once.
**Space Complexity:** O(m×n) - The solution allocates a full m×n DP table.

### vs. optimal
Your approach is algorithmically optimal in time complexity since any solution must examine each cell at least once. However, the space complexity can be optimized to O(min(m, n)) by using only a single row/column for DP storage instead of the full grid.

### Improvements
1. **Space Optimization:** Instead of maintaining a full 2D DP table, use a 1D array of size n (number of columns) and update it row by row:
   ```cpp
   vector<int> dp(m, 0);
   dp[0] = grid[0][0];
   for (int j = 1; j < m; j++) 
       dp[j] = dp[j-1] + grid[0][j];
   for (int i = 1; i < n; i++) {
       dp[0] += grid[i][0];
       for (int j = 1; j < m; j++)
           dp[j] = min(dp[j-1], dp[j]) + grid[i][j];
   }
   return dp[m-1];
   ```

2. **Redundant Checks:** The inner loop's conditionals `if (i-1>=0)` and `if (j-1>=0)` are unnecessary for j > 0 since the first row and first column are precomputed separately.

3. **Code Clarity:** Merge the initial row initialization into the main loop logic to avoid separate loops. Use more descriptive variable names (e.g., `rows`/`cols` instead of `n`/`m`).

### Why the percentile is low
The runtime percentile is low because this solution uses O(m×n) extra space while faster submissions optimize space to O(min(m, n)) or even O(1) by modifying the input grid in-place. Memory allocations for the DP table are expensive, and avoiding them improves cache performance and reduces overhead.