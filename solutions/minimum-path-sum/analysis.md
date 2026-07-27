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
The submission attempts dynamic programming but incorrectly uses `max` instead of `min` when computing path costs—this is a critical logic error. However, the code coincidentally passed the test cases because all entries were non-negative and the grid was small, causing `max` to often pick the same paths as `min` in the given samples. With proper test input, this code would fail. The intended approach is correct (2D DP), but implementation is flawed.

### Complexity
- **Time complexity**: O(m×n) due to nested loops over the grid.
- **Space complexity**: O(m×n) for the DP matrix.

### vs. optimal
The optimal approach uses DP with the recurrence dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1]) for i>0 and j>0. This submission’s use of `max` instead of `min` is incorrect and would produce wrong answers on many valid inputs (e.g., grids with varied values).

### Improvements
1. **Critical bug**: Change `max` to `min` in both inner conditionals. Additionally, the initialization of `x` to 0 and adding grid cost creates incorrect logic for non-zero start.
2. **Unnecessary inner loop**: The inner loop’s condition `if(i-1>=0)` is always true when i≥1; simplify logic directly using dp[i-1][j] and dp[i][j-1].
3. **Redundant variable and initialization**: Instead of `int x=0;`, directly compute the min of top and left dp values (after handling edges). You also incorrectly initialize the first column in the double loop; first column should be handled separately like the first row.

### Why the percentile is low
Even with the bug fixed, the solution uses O(m×n) extra space. Faster solutions reduce space to O(n) by reusing a 1D DP array (since only previous row and left cell matter). Others modify the input grid in-place to avoid extra allocation entirely. The inefficient logic here and extra condition checks also hurt runtime, though asymptotically similar.