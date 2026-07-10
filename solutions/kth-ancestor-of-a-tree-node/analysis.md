---
problem: "Kth Ancestor of a Tree Node"
difficulty: unknown
verdict: Accepted
runtime: 25 ms
memory: 116 MB
date: 2026-07-10
---

# Analysis

### Verdict summary  
The solution uses binary lifting with a dynamic programming table `dp[i][j]` that stores the `(2^i)`-th ancestor of node `j`. This is the standard optimal approach for static-tree ancestor queries and fits the constraints well.

### Complexity  
**Time:** `O(n log n)` preprocessing (constructor) and `O(log n)` per query.  
**Space:** `O(n log n)` for the DP table.

### vs. optimal  
This is the optimal asymptotic complexity for this problem. Binary lifting is the known standard here; it balances preprocessing and query time appropriately for up to `5*10^4` nodes and queries.

### Improvements  
1. **LOG calculation** is off by one: `while((1<<LOG) <= n)` makes LOG = ⌈log₂(n)⌉, but you only need ancestors up to `n-1` steps. Actually, since `k ≤ n`, a tighter bound is `while((1<<LOG) <= n)` still works but a simpler `LOG = __lg(n) + 1;` is clearer and avoids looping.  
2. **DP table indexing:** `dp[i][j] = dp[i-1][dp[i-1][j]]` assumes `dp[i-1][j]` is valid; else you store -1 anyway and check later. This is fine, but you could precompute `LOG = ceil(log2(n))` without the `+1` shift to save one row if desired.  
3. **Initialization loop efficiency:** The first loop `for i = 0` just copies `parent` into `dp[0]`. This could be done directly with `dp[0] = parent`, but careful since `parent` is `vector<int>&` and may be modified later – copying is safer.  
4. **Query loop condition:** `ans != -1` could be omitted inside the loop in binary lifting – once ancestor is -1, it stays -1. But leaving it doesn’t hurt correctness.