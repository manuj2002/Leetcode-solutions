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
The submission attempts a constant space solution by using the first row and column as markers, but it incorrectly stores the original values of the first row and column in separate vectors, which uses O(m+n) space. The core idea is correct, but the implementation is flawed and not constant space.

### Complexity
Time: O(m*n), doing multiple passes through the matrix.  
Space: O(m+n), due to storing `row` and `col` vectors.

### vs. optimal
The optimal solution uses the first row and column as markers without extra storage. It first checks if the first row/column should be zeroed, then uses the rest of the matrix to set markers in the first row/column, processes the inner matrix based on markers, and finally zeroes the first row/column if needed. This achieves O(1) space. This submission uses extra vectors, which is inefficient.

### Improvements
1. Remove the `row` and `col` vectors. Instead, use two boolean flags to track if the first row and first column should be zeroed.
2. Use the first row and column as markers directly. For example:  
   - Check if first row has any zero (flag `firstRowZero`).  
   - Check if first column has any zero (flag `firstColZero`).  
   - For each cell (i, j) where i>0 and j>0, if it is zero, set `matrix[i][0] = 0` and `matrix[0][j] = 0`.  
   - Zero rows and columns based on markers, excluding the first row/column.  
   - Zero the first row and column if the flags are set.
3. The current code has a bug: it uses `==` (comparison) instead of `=` (assignment) in the last two loops. This is a critical error that would break the solution.

### Why the percentile is low
The solution uses O(m+n) space instead of O(1), and the incorrect use of `==` would cause it to fail (though it passed due to undefined behavior). Faster solutions use true constant space by leveraging the matrix itself for storage without auxiliary arrays. Additionally, the multiple passes and redundant checks (like storing the entire first row/column) add unnecessary overhead.