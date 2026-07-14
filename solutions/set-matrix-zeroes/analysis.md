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
The submission implements an in-place solution using the first row and first column as markers to track which rows and columns need to be zeroed. This is the correct constant-space approach for the problem. However, there are inefficiencies in the implementation that affect performance.

### Complexity
- **Time complexity**: O(m × n) — The solution performs multiple passes over the matrix, but each pass is linear in the size of the matrix.
- **Space complexity**: O(1) — Only a few integer variables are used, satisfying the constant-space requirement.

### vs. optimal
The optimal approach for this problem is indeed O(m × n) time and O(1) space, using the first row and first column as marker arrays. This submission follows that strategy. However, the optimal implementation avoids redundant checks and minimizes the number of passes. Specifically, the marker-checking loops here are slightly inefficient in their structure.

### Improvements
1. **Merge initial marker scans**: The first two loops checking `isfirstROw` and `isfirstCol` can be combined into the main marking loop. Currently, they are separate passes.  
2. **Eliminate redundant inner loop**: The second nested loop has an inefficient structure—iterating over columns in the outer loop and rows in the inner loop is cache-inefficient. It should iterate over rows first, then columns.  
3. **Variable naming and initialization**: Use booleans (`bool`) instead of integers for flags (`isfirstROw`, `isfirstCol`). Initialize them to `false` and set to `true` upon finding a zero.  
4. **Remove unnecessary loop nesting**: The zero-setting loop after marking can be simplified to two separate loops for rows and columns, which is clearer and more efficient.

### Why the percentile is low
Despite being O(1) in space, the runtime is impacted by:
- **Cache-inefficient loops**: The column-major inner loop in the zero-setting phase causes poor cache locality.
- **Extra passes**: The initial separate scans for the first row/column add unnecessary operations. Top solutions merge these checks into the main marking pass.
- **Non-idiomatic logic**: Using integer counts for flags and inverted loop orders are minor but contribute to overhead. The fastest solutions use tighter, cache-friendly loops and minimal branching.