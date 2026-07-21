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
The submission uses a standard backtracking approach to solve the N-Queens problem, which is fundamentally correct. However, the implementation contains significant logical errors in the collision detection and backtracking logic that would prevent it from finding all solutions. The code as shown would not actually work correctly—particularly the diagonal checks that lack `return` statements and the premature backtracking logic.

### Complexity
- **Time complexity**: O(N!) in theory, but due to flawed diagonal checking (which doesn't actually return false when it should), the true complexity is undefined. With proper fixes, it would be O(N!).
- **Space complexity**: O(N²) for the board representation, plus O(N) for recursion depth.

### vs. optimal
The optimal approach for N-Queens uses backtracking with efficient O(1) collision checks using hash sets for columns and diagonals. This implementation attempts that but fails in execution: the diagonal checks are broken (missing `return` statements), and the backtracking logic incorrectly terminates early when a row has no solution (the `if(!next) return false` part), which prevents finding all solutions.

### Improvements
1. **Fix diagonal collision checks**: The diagonal while-loops detect a collision but don’t return false (just evaluate `false` without returning). Add `return false` after each detection.
2. **Remove early termination**: Delete the `if(!next) return false` block—it incorrectly stops the search after the first failing row, preventing exploration of other columns.
3. **Use sets for O(1) checks**: Replace linear scans with unordered_sets for columns and diagonals to reduce collision checks from O(N) to O(1).
4. **Simplify board initialization**: Use `vector<string> curr(n, string(n, '.'))` for cleaner initialization.

### Why the percentile is low
The code appears accepted but likely passed due to weak test cases or was fixed after submission (the shown code has critical bugs). Faster solutions use:
- Bitmasking to represent threats in O(1) time.
- Iterative backtracking to avoid recursion overhead.
- Diagonal indexing via `row-col` and `row+col` stored in sets for instant checks.
This implementation uses slower O(N) checks per placement and has flawed logic that would hinder performance on larger N.