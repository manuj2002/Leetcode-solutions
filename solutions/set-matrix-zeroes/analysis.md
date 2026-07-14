---
problem: "Set Matrix Zeroes"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-14
---

# Analysis

### Verdict Summary
Your solution correctly uses the first row and first column as markers to record which rows and columns need to be zeroed, which is a standard constant-space approach. However, the implementation has a flaw in the way it handles the first row and column, leading to redundant checks and suboptimal performance.

### Complexity
- **Time Complexity:** O(m * n) — The code iterates through the matrix multiple times, but each pass is linear in the number of elements.
- **Space Complexity:** O(1) — Only a few integer variables are used, so it is constant space.

### vs. Optimal
The optimal approach for this problem uses the first row and first column as markers to avoid extra space. Your solution does this, but the implementation is inefficient. The optimal method typically checks the first row and column first, then uses them to mark the inner matrix, and finally zeroes the inner matrix based on those markers before handling the first row and column. Your code has an extra loop for the first column and uses nested loops in a suboptimal way when applying the zeros.

### Improvements
1. **Redundant First Column Check:** The initial loops to check for zeros in the first row and column are unnecessary. Instead, you should traverse the entire matrix and use the first row and column as markers, but only after storing whether the first row and column themselves need to be zeroed. This avoids two full passes.
2. **Inefficient Zero Application:** The loops that zero the rows and columns (lines 26-40) are inefficient. Specifically, the inner loop for columns (lines 33-39) is nested incorrectly; it should iterate over rows for each column, but you have it iterating over columns for each row. This causes redundant checks. Instead, zero the inner matrix based on the markers in the first row and column in two separate passes.
3. **Idiomatic C++:** Use `bool` for flags (`isfirstROw`, `isfirstCol`) instead of `int` to semantically represent boolean values.

### Why the Percentile is Low
The solution is correct but not optimally implemented. The extra initial passes and the nested loop structure when zeroing the matrix introduce unnecessary operations. Faster solutions avoid these by storing the state of the first row and column in two boolean variables during the first pass, then marking the inner matrix, zeroing the inner matrix based on the markers, and finally zeroing the first row and column if needed. This reduces the number of passes and checks, making it more efficient.