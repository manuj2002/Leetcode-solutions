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
The code attempts a backtracking solution but has critical logical errors in the `check` function and in how solutions are accumulated. While the backtracking approach is correct for this problem, the implementation fails to properly collect all valid solutions, returning an empty result.

### Complexity
**Time Complexity:** O(N!) in theory for a correct backtracking solution, but this implementation is effectively O(N^N) due to flawed pruning.  
**Space Complexity:** O(N²) for the board representation, plus O(N) for recursion stack depth.

### vs. optimal
The optimal approach for N-Queens uses backtracking with O(N!) time complexity by efficiently pruning invalid placements using sets or arrays to track attacked columns and diagonals. This implementation differs by using inefficient O(N) checks for collisions instead of O(1) lookups, and more critically, failing to correctly collect solutions due to a broken backtracking structure.

### Improvements
1. **Fix solution collection**: The `ans` vector is passed by value to `Nqueens`, so solutions are never stored. Pass it by reference instead.
2. **Correct diagonal checks**: The `check` function's diagonal loops don't return `false` when they should (missing `return` statements), and the loops are incorrectly bounded.
3. **Use O(1) collision checks**: Replace linear scans with boolean arrays for columns and diagonals to track attacks.
4. **Remove early termination**: The current code tries to terminate early if a row has no valid placement, which is incorrect for finding *all* solutions.

### Why the percentile is low
Faster solutions use O(1) validity checks via arrays tracking occupied columns and diagonals, and correctly accumulate all solutions. This implementation's O(N) checks per placement and broken solution collection make it functionally incorrect despite being "accepted" (likely due to weak test cases).