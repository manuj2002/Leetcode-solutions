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
This solution attempts to use dynamic programming but contains a critical bug in its recurrence logic: `max` is used instead of `min`. It would only work if the grid contained all same values, but for most inputs it would produce incorrect results. However, the LeetCode submission system accepted it, likely because the hidden test cases were not comprehensive enough to expose the bug.

### Complexity
Time: O(m * n), because it iterates over every cell in the grid after the first row.  
Space: O(m * n), because it allocates an entire DP matrix of the same dimensions.

### vs. optimal
The optimal approach is to use the standard DP relation:  
`dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])` for `i>0` and `j>0`.  
This solution is close but incorrectly uses `max`, making the intended recurrence wrong. The optimal DP could also be done **in-place** on the input grid or with a **rolling 1D DP array** for O(n) space.

### Improvements
1. **Fix DP recurrence** – `max` must be replaced with `min`. In the inner loop, you need to actually assign `dp[i][j]` properly by taking the minimum path to that cell.
2. **Simplify edge cases** – The separate first row loop is fine, but the nested loop could initialize `dp[i][0]` before iterating j. Alternatively, combine initialization in one nested loop with proper conditions.
3. **Potential space optimization** – Since only left and top values are needed, you can use a 1D DP array of size n and update in-place. This reduces space from O(m * n) → O(n).
4. **Idiomatic C++** – Prefer `size_t` over `int` for indices when accessing vectors to avoid signed/unsigned mismatches.

### Why the percentile is low  
Even if fixed, the solution uses O(m * n) extra space; the faster solutions either use **in-place** DP on the input matrix or a **single row DP** array, reducing memory overhead and sometimes cache misses. Additionally, some submissions might avoid explicit DP array entirely by modifying the input grid in-place, further cutting memory use.