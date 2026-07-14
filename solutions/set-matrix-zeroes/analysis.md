---
problem: "Set Matrix Zeroes"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
This solution uses O(m + n) extra space to record rows and columns containing zeros, then iterates to set those rows and columns to zero. While functionally correct, it is not the optimal space solution for this problem, which can be solved in O(1) auxiliary space.

### Complexity
- **Time complexity:** O(m × n) — Two nested loops over the matrix (first to find zeros, second to set rows/columns).
- **Space complexity:** O(m + n) — Two vectors (`row` and `col`) store up to m rows and n columns.

### vs. optimal
The optimal solution uses O(1) space by leveraging the first row and first column of the matrix as markers for which rows/columns need to be zeroed. It first checks if the first row/column itself needs to be zeroed, then uses them as flags, and finally processes the matrix based on those flags. This solution uses extra vectors instead, missing the constant-space technique.

### Improvements
1. **Eliminate auxiliary vectors:** Use the first row and column as flags to store zero information, reducing space to O(1).
2. **Avoid redundant loops:** Merge the zero-setting passes by processing markers in place.
3. **Early termination:** If no zeros are found, return early to save cycles (minor optimization).
4. **Use boolean flags:** For clarity, use boolean variables to track if the first row/column should be zeroed.

### Why the percentile is low
The runtime/memory percentiles are low because this solution uses O(m + n) space, while the fastest submissions use the O(1) in-place marking technique. The extra space allocation and separate passes for row/column zeroing add overhead, making it less efficient compared to optimal implementations.