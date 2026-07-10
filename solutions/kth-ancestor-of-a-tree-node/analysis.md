---
problem: "Kth Ancestor of a Tree Node"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.5 MB
date: 2026-07-10
---

# Analysis

### Verdict summary
The solution correctly implements binary lifting to handle kth ancestor queries efficiently. It precomputes ancestors at powers of two and walks up the tree in O(log n) per query, which is the expected approach for this problem.

### Complexity
- **Time:** Construction O(n log n), Query O(log k) (or O(log n) in the worst case).
- **Space:** O(n log n), from the `dp` table of size (LOG+1) × n.

### vs. optimal
This **is optimal** for this problem. The binary lifting technique is the standard solution for kth ancestor queries online, with O(n log n) preprocessing and O(log n) per query, matching known optimal bounds.

### Improvements
1. **LOG calculation efficiency:** `while ((1 << LOG) <= n)` can be replaced with `LOG = __lg(n) + 1;` (using bit width) or `LOG = floor(log2(n)) + 1;` to compute it directly instead of a loop.
2. **Inconsistent LOG usage:** Setting `LOG` to `ceil(log2(n))` is sufficient; currently the loop increments until `(1 << LOG) > n`, then uses `LOG` unchanged. The final `(1 << LOG)` exceeds n, but the code still loops to `<= LOG` in queries. This works but is slightly larger than needed. Precisely, `LOG = __lg(n) + 1;` is enough.
3. **Potential minor speed-up:** In `getKthAncestor`, iterate over bits while checking `k` directly: `if (k & (1 << i)) ans = dp[i][ans];` instead of subtracting. This avoids modifying `k` and is clearer.

### Why the percentile is low
The runtime and memory values provided are "beats —" (i.e., no percentile shown). If slow, common micro-optimizations include:
- Allocating `dp` as a single vector of size (LOG+1)*n and computing indices manually to reduce allocation overhead.
- Using `int` instead of `vector<int>` for each row if LOG is small.
- Using `std::bitset` or iterative bit checks without subtraction in the query loop for branch prediction.
- Using `parent` directly as the first row to avoid copy (but the assignment `dp[0][i]=parent[i]` already copies). If parent is already stored separately, you could skip `dp[0]` by referencing it dynamically, but that adds checks.