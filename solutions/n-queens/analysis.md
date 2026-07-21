---
problem: "N-Queens"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.1 MB
date: 2026-07-21
---

# Analysis

### Verdict summary
The code uses backtracking with a correct overall strategy but contains a critical bug in the diagonal collision checks that prevented it from finding all solutions. While the row-by-row placement approach is standard, the implementation fails to validate the upward diagonals correctly due to missing return statements in the collision detection logic.

### Complexity
**Time Complexity:** O(N!) in the worst case, though pruning occurs. The `check` function runs in O(N) per call.  
**Space Complexity:** O(N²) for the board storage, plus O(N) for the recursion stack.

### vs. optimal
The optimal approach for N-Queens uses backtracking with O(1) collision checks via sets or bitmasking for diagonals. This solution attempts the same backtracking strategy but is suboptimal due to:
1. **Inefficient collision checks:** The `check` function uses O(N) scans instead of O(1) lookups.
2. **Incorrect pruning:** The code returns `false` after the first failed row (line 36), which aborts exploration prematurely. This is a logic error, not a valid optimization.

### Improvements
1. **Fix the diagonal checks:** Lines 19 and 26 are missing `return false` after detecting a collision. They should read:
   ```cpp
   if(curr[i1][j1]=='Q') return false;
   ```
2. **Remove premature backtracking:** Change lines 35–38 to always continue searching after backtracking:
   ```cpp
   curr[i][j]='Q';
   Nqueens(i+1,ans,curr,n); // Remove bool assignment
   curr[i][j]='.';
   ```
3. **Optimize collision tracking:** Use boolean arrays for columns and diagonals to reduce `check` to O(1):
   ```cpp
   vector<bool> col(n), diag1(2*n-1), diag2(2*n-1);
   ```
4. **Use a more idiomatic board initialization:**
   ```cpp
   vector<string> curr(n, string(n, '.'));
   ```

### Why the percentile is low
Faster solutions use O(1) collision checks with arrays or bit masks instead of O(N) scans. This implementation’s diagonal checks are both inefficient and broken, causing redundant work and incorrect results. The premature termination in `Nqueens` further reduces the number of solutions found, though the code passed due to weak test cases.