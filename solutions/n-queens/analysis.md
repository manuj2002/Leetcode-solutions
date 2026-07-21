---
problem: "N-Queens"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-21
---

# Analysis

### Verdict summary
The submission attempts to use backtracking, which is the correct approach for solving the N-Queens problem. However, the implementation contains several critical syntax and logic errors that prevent it from compiling and running correctly.

### Complexity
**Time Complexity:** If implemented correctly, the backtracking algorithm would have a theoretical worst-case time complexity of O(n!), as it explores all possible queen placements row by row, pruning invalid branches.  
**Space Complexity:** The space complexity would be O(n²) for the board representation and O(n) for the recursion stack.

### vs. optimal
The backtracking approach is optimal for this problem, as it is the standard solution for N-Queens. However, the implementation here is flawed. The optimal approach uses backtracking with efficient collision checks for queens in the same column and diagonals, typically using sets or arrays to track attacked columns and diagonals in O(1) time per check.

### Improvements
1. **Syntax Error Fix:** Line 46 (`curr[i][j]='.'`) is missing a semicolon. This is the immediate cause of the compile error.  
2. **Logical Error in Diagonal Checks:** The diagonal checks in the `check` function are incorrect. They only check one direction per while-loop and fail to return `false` when a queen is found (e.g., `false;` should be `return false;`).  
3. **Backtracking Logic Flaw:** The `Nqueens` function incorrectly returns `false` after a failed branch, which would prematurely terminate the search. It should backtrack by resetting the queen placement and continue searching (`next` is unnecessary; just revert `curr[i][j]` and proceed).  
4. **Inefficient Collision Checks:** The current checks scan entire rows and columns unnecessarily. Use arrays to track attacked columns and diagonals for O(1) checks instead of O(n) scans.  
5. **Code Readability:** The diagonal loops have redundant bounds checks (`j1>=0 && j1<n` is always true for `j1` starting from `j`). Simplify logic and add comments for clarity.