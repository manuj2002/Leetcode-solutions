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
The submission attempts binary lifting (jumping ancestors in powers of two) but has critical indexing errors. The core idea is appropriate for the problem, but the implementation is flawed.

### Complexity  
- **Time:** Constructor *O(n log n)*, getKthAncestor *O(log n)* — correct for binary lifting.  
- **Space:** *O(n log n)* — standard for the DP table.

### vs. optimal  
The binary lifting approach is optimal for this problem (time *O(log n)* per query, space *O(n log n)*). However, the code’s DP table construction is wrong: it accesses `dp[i-1][j]` where `dp[i-1][j]` could be `-1`, causing out-of-bounds vector access later and an eventual overflow error.

### Improvements  
1. **Index bounds check** — the line `dp[i][j] = dp[i-1][dp[i-1][j]]` must first ensure `dp[i-1][j] != -1`; else keep `dp[i][j] = -1`.  
2. **LOG calculation** — `while((1<<LOG) <= n)` actually computes `LOG = floor(log2(n)) + 1`, but the loop in `getKthAncestor` should iterate from `LOG-1` down to `0`, not from `1<<LOG` (which is ~n). Use `for(i = LOG; i >= 0; i--)` with `if(k & (1<<i))`.  
3. **Off-by-one in DP dimensions** — DP size `LOG+1` is fine, but initialize rows for `i=1..LOG-1` not `LOG`? Actually `i` should go to `LOG-1` in constructor if 0‑indexing `dp[0]` as direct parents.  
4. **Use of (1<<i) in query loop** — `1<<i` when `i = LOG` can be larger than `INT_MAX` if `LOG` large; safer to define `MAX_LOG = ceil(log2(n))` and iterate `for(int i = MAX_LOG; i >= 0; i--)`.  
5. **Redundant storage of `dp[0][j]`** — You could store `dp[0] = parent`, but careful with `-1` as done.