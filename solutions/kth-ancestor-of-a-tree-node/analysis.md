---
problem: "Kth Ancestor of a Tree Node"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.8 MB
date: 2026-07-10
---

# Analysis

### Verdict summary
This solution correctly implements binary lifting, an optimal approach for repeatedly querying k-th ancestors. It preprocesses the tree into a dynamic programming table where `dp[i][j]` stores the `2^i`-th ancestor of node `j`. This allows `getKthAncestor` to answer queries in logarithmic time by decomposing `k` into powers of two.

### Complexity
- **Time:** Constructor: O(n log n), `getKthAncestor`: O(log n) per query
- **Space:** O(n log n) for storing the DP table

### vs. optimal
This IS the optimal approach for the problem constraints. The binary lifting technique achieves the best possible trade-off between preprocessing time and query time when handling up to 5×10⁴ queries.

### Improvements
1. **LOG calculation**: The condition `while((1<<LOG)<=n)` can overshoot. Better to use `while((1<<LOG) < n) LOG++;` to avoid an unnecessary extra row when `n` is a power of two.
2. **Binary representation**: Instead of checking `if((1<<i)<=k)` in the query, use bitmask checking with `if(k & (1<<i))` which avoids modifying `k` and is more idiomatic.
3. **Loop optimization**: The inner preprocessing loop can be optimized by iterating only over valid nodes, though this doesn't change asymptotic complexity.

### Why the percentile is low
Despite being optimal, the relatively low memory percentile (8.8MB) suggests the implementation uses more memory than necessary. Top solutions likely:
- Use a more compact representation (e.g., 2D array vs vector of vectors)
- Better handle edge cases in the LOG calculation to minimize table size
- May use iterative depth-first search for preprocessing to optimize cache locality

The runtime percentile (0ms) is high, confirming the algorithmic approach is sound. The memory usage could be improved with more careful allocation.