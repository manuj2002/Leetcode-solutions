---
problem: "N-Queens"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-21
---

# Analysis

### Verdict summary
This submission attempts to solve the N-Queens problem using backtracking, which is the correct approach. However, the implementation contains critical logical errors in the `check` function, failing to correctly validate queen placements along diagonals and columns. The code will not produce correct solutions.

### Complexity
**Time Complexity:** The code attempts backtracking but with an incorrect pruning condition. The flawed `check` function runs in O(n) per call, but since the recursion is broken prematurely (due to returning `true` after one valid path), it fails to explore all possibilities. Actual runtime is undefined due to logical errors.

**Space Complexity:** O(n²) for storing the board state, which is standard. However, the recursion stack depth is O(n), which is acceptable.

### vs. optimal
The optimal approach uses backtracking with efficient validation using three boolean arrays for columns and two diagonals (O(1) per check). This code’s validation is flawed: it only checks for queens in the same row/column? and has incorrect diagonal checks (e.g., it only scans upward-left and upward-right but misses downward directions and doesn’t account for the current row). It also stops after finding one solution per branch due to `return true` in `Nqueens`, instead of collecting all solutions.

### Improvements
1. **Fix validation logic:** The `check` function should verify:
   - No queen in the same column (for rows above current).
   - No queen on the same diagonal (both left and right) for rows above.
   Currently, it checks the current row/column? and has broken diagonal loops.
2. **Remove early termination:** `Nqueens` returns `true` after one solution, but the problem requires all distinct solutions. Change it to backtrack fully without returning early.
3. **Optimize validation:** Use arrays to track occupied columns and diagonals for O(1) checks instead of O(n) loops.
4. **Code clarity:** Avoid global-like state; pass necessary parameters (e.g., column/diagonal trackers) recursively.

### Why the percentile is low
The code is fundamentally incorrect and would fail most test cases. Correct solutions use efficient validation and proper backtracking to explore all possibilities. This implementation misses solutions due to broken checks and premature termination.