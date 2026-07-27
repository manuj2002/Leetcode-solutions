---
problem: "Assign Cookies"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-27
---

# Analysis

### Verdict summary
The submission attempts a greedy two-pointer solution from the largest greed and cookie sizes, which is the correct high-level approach. However, it contains critical out-of-bounds access errors: the loop condition `while(i>=0 || j>=0)` and unchecked pointer use cause invalid vector indexing, leading to the runtime error.

### Complexity
Time: O(g log g + s log s) for sorting, plus O(min(g, s)) for pointer passes.  
Space: O(1) aside from sorting space. The constant extra space is fine.

### vs. optimal
The optimal approach is indeed greedy with sorting and two pointers. The standard solution sorts both arrays and uses two pointers starting from the smallest elements (or largest) to match cookies to children. The difference is that the submission attempts to start from the largest elements but fails to properly guard pointer access before comparing or using indices.

### Improvements
1. **Fix out-of-bounds access**: Change the loop condition to `while(i>=0 && j>=0)` so both indices are valid when accessed. Additionally, move the pointer decrement logic to avoid using invalid `g[i]` or `s[j]` after bounds change.
2. **Match logic clarity**: Starting from smallest elements is simpler (child pointer increments when satisfied), but either direction works if bounds are handled. For largest-first, the fixed loop would be:
   ```cpp
   while (i >= 0 && j >= 0) {
       if (g[i] <= s[j]) { ans++; j--; }
       i--;
   }
   ```
3. **Use `int` for indices, avoid unsigned overflow issues**: The error mentions unsigned offset overflow; using `int` for `i`, `j` as in the code is fine, but ensure indices are non-negative before accessing. The runtime error came from accessing `g[i]` when `i` was `-1` or `j` was `-1` when checking `g[i]<=s[j]`.