---
problem: "Set Matrix Zeroes"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-14
---

# Analysis

### Verdict summary
The submission attempts an in-place approach by using the first row and column as markers, which is the correct constant-space strategy. However, the implementation contains critical syntax errors and logical flaws that prevent compilation and correct execution.

### Complexity
- **Time complexity**: O(m × n) from iterating through the matrix multiple times, which is optimal.
- **Space complexity**: O(1) extra space attempted, but O(m + n) actually used due to unnecessary vectors `col` and `row`.

### vs. optimal
The optimal approach uses the matrix's first row and first column as flags to mark which rows/columns need zeroing, avoiding extra space. Your solution attempts this but introduces significant issues:

1. **Extra O(m + n) space**: You allocate vectors `col` and `row`, contradicting the constant-space goal.
2. **Incorrect marker logic**: You overwrite the first row/column with markers before checking if they originally contained zeros, losing critical information. The correct approach checks the first row/column for zeros first, then uses them as markers, and finally zeroes them if needed.

### Improvements
1. **Fix syntax errors**: Line 10 has `for(int i=0;j<matrix[0];j++)` — should be `for(int j=0; j<matrix[0].size(); j++)`. Also, remove extra braces after `if(isfirstCol)` and `if(isfirstROw)` blocks.
2. **Eliminate redundant storage**: Remove `col` and `row` vectors entirely. Instead, check the first row/column for zeros upfront using two boolean flags (e.g., `bool firstRowZero = false`, `bool firstColZero = false`).
3. **Correct marker placement**: After checking the first row/column, iterate from `i=1` and `j=1` to mark zeros in the first row/column. Then zero rows/columns based on markers, and finally zero the first row/column if the flags are set.
4. **Fix assignment vs. comparison**: Lines 47, 49, 53, and 55 use `==` (comparison) instead of `=` (assignment).