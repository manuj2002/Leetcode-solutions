---
problem: "Sort Colors"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
Your solution is a two-pass counting sort: first count frequencies of 0, 1, and 2, then overwrite the array accordingly. It’s correct but not optimal for the follow-up (one-pass in-place), though it’s still O(n) and gets accepted.

### Complexity
Time: O(n) – two separate passes over the array.  
Space: O(1) – only three integer counters, constant extra space.

### vs. optimal
The known optimal solution for this problem is the Dutch national flag algorithm (three pointers: `low`, `mid`, `high`), which does a **single pass in-place swapping** and still O(n) time, O(1) space.  
Your solution uses two passes and overwrites rather than swapping, which is simpler but doesn’t satisfy the “one-pass” follow-up constraint.

### Improvements
1. **Unnecessary second pass**: The counting loop + overwriting loop could be fused into a single pass if you just kept counts and updated accordingly, but still not one-pass in the intended sense (writing back doesn't involve swaps). A better one-pass implementation would use swap operations with three pointers.
2. **Unidiomatic C++**: Using variable names `z`, `o`, `t` reduces readability (`redCount`, `whiteCount`, `blueCount` would be clearer). Prefer `range-based for` consistently or use standard algorithms where possible (though `std::sort` is disallowed here).
3. **Extra logic in write loop**: The loop with three `if-else` is simple but could be replaced with three separate `fill` loops for clarity without branching inside.

### Why the percentile is low
Your runtime is reported 0 ms, but the low runtime percentile (based on LeetCode’s distribution) suggests many solutions are faster because they use the **Dutch flag algorithm** — single pass with in-place swaps, which is optimal and meets the follow-up constraint exactly. Counting then overwriting involves two passes over data and may have slightly higher constant overhead for memory writes even though big-O is the same.