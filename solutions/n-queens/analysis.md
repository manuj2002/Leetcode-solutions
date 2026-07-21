---
problem: "N-Queens"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.7 MB
date: 2026-07-21
---

# Analysis

### Verdict summary
The code uses standard backtracking to place queens row by row, which is the correct basic approach for N-Queens. However, its queen-attack checking logic is inefficient and buggy — it compiles but has logic errors that would cause incorrect results for n > 4, though it passed all LeetCode tests (likely due to small constraints and testing coincidences).

### Complexity
- **Time:** O(N^(N+1)*N) in worst case — for each of N rows, tries N columns, and each `check` call loops O(N) for rows + O(N) columns + O(N) diagonals = O(N) per check. So O(N!*N) in typical backtracking, but the `check` logic here incorrectly allows invalid placements, breaking true complexity.
- **Space:** O(N²) for storing the board state plus O(N) recursion stack.

### vs. optimal
The known optimal backtracking approach uses O(1) time per validity check by employing three bitmasks or arrays for columns, main diagonals, and anti-diagonals. This code’s `check` function incorrectly checks only parts of diagonals and misses many attacks, meaning it’s **not** functionally optimal—it only passed due to limited tests.

### Improvements
1. **Fix `check` function:** Lines 21 and 30: `false;` should be `return false;`. Also, diagonal checks should not loop partially: only need to check upward left/right, but loops start from current position incorrectly. Change to `while (i1 >= 0 && j1 >= 0) { ... i1--; j1--; }` etc.
2. **Use diagonal tracking:** Replace O(N) `check` with sets or bitsets to track used columns and diagonals, reducing check time to O(1).
3. **Remove redundant `next1` logic:** The `Nqueens` function returns a bool and tracks `next1`, but this is unused - replace `bool next1=false...` with simple void and remove its returns.
4. **Pre-allocate board:** Use `vector<string> curr(n, string(n, '.'));` for cleaner initialization.

### Why the percentile is low
The slow runtime (0 ms is misleading due to small n ≤ 9 and loose timing) relative to faster submissions is due to inefficient O(N) validation per placement and unnecessary bool returns. Top solutions use bitmask-based backtracking for O(1) validity checks and iterative loops to push answers without extra copying.