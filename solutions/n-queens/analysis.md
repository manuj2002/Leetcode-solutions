---
problem: "N-Queens"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 10.2 MB
date: 2026-07-21
---

# Analysis

### Verdict summary
The solution uses backtracking to explore all valid queen placements row by row, which is the standard approach for N-Queens. The implementation correctly generates all distinct solutions and is efficient enough for the constraint n ≤ 9.

### Complexity
**Time complexity:** O(N!) — The backtracking explores up to N choices in the first row, N-1 in the second (minus invalid ones), etc., leading to factorial growth.  
**Space complexity:** O(N²) — The `curr` board state is stored as N strings of length N, and the recursion stack goes up to depth N.

### vs. optimal
This solution is optimal for generating all distinct configurations. The standard optimal approach uses backtracking with three boolean arrays (for columns and two diagonals) to check validity in O(1) time per cell. This implementation uses O(N) time per check, which is acceptable for n ≤ 9 but would not scale well for larger n.

### Improvements
1. **Use arrays for O(1) validity checks:** Replace the O(N) `check` function with three boolean arrays (for columns, main diagonals, and anti-diagonals) to track occupied lines. This reduces per-place checks from O(N) to O(1).
2. **Avoid string reconstruction:** Preallocate the board as a vector of strings once and reuse it, but avoid building strings row-by-row in `solveNQueens`. Instead, initialize `curr` with n strings of n '.' characters directly.
3. **Remove redundant backtracking return value:** The function `Nqueens` returns a bool, but this is unused in the solution generation. Since all solutions are needed, not just the first, the return value is unnecessary and can be made void.
4. **Optimize diagonal indexing:** For diagonal arrays, use indices `i + j` for anti-diagonals and `i - j + n - 1` for main diagonals to avoid the while-loop checks.