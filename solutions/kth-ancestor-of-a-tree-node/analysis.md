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
The submission implements binary lifting, which is the correct approach for efficiently answering multiple `k`-th ancestor queries. However, it contains a critical out-of-bounds access error due to improper bounds checking in the lookup loop.

### Complexity
- **Time:** Constructor O(n log n) due to building the DP table, `getKthAncestor` O(log k) per query.
- **Space:** O(n log n) for storing the DP table.

### vs. optimal
Your approach *is* optimal in theory—binary lifting is the standard solution for this problem. However, the implementation error causes runtime failure. A correct implementation would match the optimal O(n log n) preprocessing and O(log k) query time.

### Improvements
1. **Fix boundary check in lookup loop:** The main issue is in `getKthAncestor`. When `dp[i][ans]` is accessed, `ans` might be -1 (no ancestor), but the code still uses it as an index in the next iteration. Change the loop condition to break early when `ans == -1`:
   ```cpp
   for(int i = LOG; i >= 0 && ans != -1; i--) {
       if(k & (1 << i)) {
           if(ans == -1) break; // Add this check
           ans = dp[i][ans];
       }
   }
   ```

2. **Prevent invalid DP table accesses:** In the constructor, when computing `dp[i][j]`, check if `dp[i-1][j]` is valid before using it as an index:
   ```cpp
   for(int i = 1; i <= LOG; i++) {
       for(int j = 0; j < n; j++) {
           int mid = dp[i-1][j];
           dp[i][j] = (mid == -1) ? -1 : dp[i-1][mid];
       }
   }
   ```

3. **Use bitmask check idiom:** Replace `(1<<i) <= k` with `(k >> i) & 1` for clarity and to avoid potential integer overflow with large `i`.

4. **Avoid over-allocating LOG:** The calculation `while((1<<LOG)<=n) LOG++;` may overallocate. Since `n ≤ 5e4`, LOG ≤ 16. Consider setting `LOG = ceil(log2(n))` explicitly.