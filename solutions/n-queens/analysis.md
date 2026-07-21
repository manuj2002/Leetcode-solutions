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
The approach uses backtracking with a correct base case but has flawed collision checks. It is fundamentally correct but inefficient due to redundant checks and unnecessary return values.

### Complexity
**Time**: O(N!) in the worst case due to backtracking, but with an inefficient O(N) per placement check, leading to O(N * N!) overall.
**Space**: O(N²) for the board storage and O(N) for the recursion stack.

### vs. optimal
The optimal approach for N-Queens uses backtracking with O(1) per placement checks by tracking attacked columns and diagonals via boolean arrays. This reduces the per-placement check from O(N) to O(1), making the overall complexity O(N!) without the linear multiplier.

### Improvements
1. **Diagonal checking is broken**: The loops for diagonals don't break early and have a logic error (they return `false` incorrectly). They should return false immediately upon finding a queen, but currently they don't—the code has statements like `false;` that do nothing. This must be fixed to `return false`.
2. **Use arrays for attacked directions**: Instead of scanning the entire board, maintain boolean arrays for columns, primary diagonals (i+j), and anti-diagonals (i-j+n-1) to check in O(1).
3. **Remove redundant return value**: The function `Nqueens` returns a bool but doesn't use it for anything. It should be `void` since we want all solutions, not just the first.
4. **Preallocate the board**: Avoid building the initial board with string appends; use `string(n, '.')` for efficiency.

### Why the percentile is low
The solution is slower than optimal because it uses O(N) checks per placement instead of O(1). The fastest solutions use boolean arrays for columns and diagonals, eliminating the inner loops. Additionally, the broken diagonal checks might cause unnecessary backtracking or even errors, though the tests passed.