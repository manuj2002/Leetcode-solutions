---
problem: "House Robber"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.1 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
This solution uses dynamic programming with an O(n^2) inner loop, which is inefficient for the problem constraints. While it correctly computes the maximum amount by considering non-adjacent houses, the approach is suboptimal compared to the standard O(n) DP solution.

### Complexity
**Time:** O(n^2) due to the nested loop structure.  
**Space:** O(n) for the DP array.

### vs. optimal
The optimal approach uses O(n) time and O(1) space with two variables tracking the maximum profit up to the previous two houses. This solution differs by using an O(n) DP array and an unnecessary inner loop to find the maximum value from indices i-2 down to 0, which is redundant since the DP array already accumulates the maximum values.

### Improvements
1. Replace the O(n^2) inner loop with a single pass that maintains the maximum value up to i-2. The inner loop is redundant and inefficient.
2. Use two variables (e.g., `prev` and `curr`) to store state instead of a full DP array, reducing space to O(1).
3. Handle edge cases (e.g., n=1) explicitly, as the current code assumes n>=2 (accessing dp[1] without checks).

### Why the percentile is low
The faster solutions use a linear pass with constant space, updating two variables: `rob` (current house robbed) and `notRob` (current house not robbed). They avoid the inner loop by tracking the maximum profit incrementally, achieving O(n) time and O(1) space. This solution's quadratic inner loop causes unnecessary overhead, especially for n up to 100.