---
problem: "Set Matrix Zeroes"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.4 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
The approach attempts a constant-space solution by using the first row and column as markers, but it incorrectly handles the first row and column by storing their original state in separate variables that are later misapplied. The core idea is correct, but implementation flaws lead to incorrect handling of edge cases.

### Complexity
- **Time complexity:** O(m × n) from four passes over the matrix (two for storing initial state, two for zeroing rows/columns).  
- **Space complexity:** O(m + n) due to unnecessary `col` and `row` vectors, violating the constant-space goal.

### vs. optimal
The optimal solution uses the first row and column as markers without allocating extra arrays. It first checks if the first row/column should be zeroed, then uses the rest of the matrix to mark zeros in the first row/column, zeroes the inner matrix based on markers, and finally zeroes the first row/column if needed. This achieves O(1) space and O(m × n) time. The submitted code misses the mark by storing the first row/column in vectors (O(m + n) space) and misusing the `isfirstROw/isfirstCol` flags with incorrect assignment syntax (`==` instead of `=`).

### Improvements
1. **Eliminate redundant vectors:** Remove `col` and `row` vectors entirely. Use the first row/column directly for marking.
2. **Fix assignment errors:** Lines 42 and 48 use `matrix[i][0]==0` (comparison) instead of `matrix[i][0]=0` (assignment). This prevents zeroing the first row/column when intended.
3. **Simplify logic:** Check and store the first row/column zero status in boolean flags (not counters), then process the inner matrix for markers, zero rows/columns based on markers, and finally zero the first row/column if flags are true.
4. **Merge passes:** Reduce the number of passes by combining the marker-setting and zeroing steps where possible.

### Why the percentile is low
The code fails to achieve O(1) space due to unnecessary vectors, and the assignment bugs cause incorrect results for matrices where the first row or column contains zeros. Faster solutions avoid extra allocations, use tighter loops, and correctly handle the first row/column with proper in-place markers.