---
problem: "Kth Ancestor of a Tree Node"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-10
---

# Analysis

### Verdict summary
The approach correctly uses binary lifting for Kth ancestor queries, but the implementation contains critical errors: the `LOG` static variable isn't initialized, and the logic assumes valid ancestor indices when `jump = -1`. The runtime error is due to using `dp[i][ans]` when `ans = -1`.

### Complexity
**Time:** Constructor: O(n log n), Query: O(log n).  
**Space:** O(n log n) for the `dp` table.  
This matches the intended complexities for the binary lifting approach.

### vs. optimal
This problem's known optimal solution is binary jumping (binary lifting) with `dp[logMax][n]`. The complexity should be as above. However, the submitted code is not optimal because:
1. It uses an unnecessarily large loop bound in `getKthAncestor` (up to `1<<LOG` instead of `LOG`).
2. It does not handle jumps to non-existent ancestors correctly, causing illegal memory access when `node = -1` is passed as an index to `dp`.

### Improvements
1. **Initialize `LOG`** and make it non‑static: In the class, `int LOG = 0;` must be initialized. As static, it retains value across multiple test cases, causing wrong calculation.
2. **Check for -1 during jumps**: In both preprocessing and query, when `dp[i-1][j] == -1`, the next jump `dp[i][j]` should also be -1, not `dp[i-1][-1]`.
3. **Fix loop in `getKthAncestor`**: Change `for(int i=(1<<LOG);i>=0;i--)` to `for(int i=LOG;i>=0;i--)`. Otherwise, the shift exponent becomes huge/negative.
4. **Early exit in query**: If `ans` becomes -1 at any step, return -1 immediately.
5. **Use `parent.size()`**: In constructor, `n` can be replaced by `parent.size()` to avoid param duplication.