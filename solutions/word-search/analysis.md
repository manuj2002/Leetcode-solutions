---
problem: "Word Search"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.6 MB
date: 2026-07-24
---

# Analysis

### Verdict summary
This solution uses DFS with backtracking to search for the word in the grid, which is the correct approach for this problem. The implementation correctly marks visited cells and backtracks, but has some inefficiencies in control flow.

### Complexity
**Time Complexity**: O(m*n*4^L) where m and n are grid dimensions and L is word length. In worst case, DFS explores 4 directions at each step up to L depth.
**Space Complexity**: O(L) for recursion stack depth (no additional space beyond the modified board for marking visited cells).

### vs. optimal
This is the optimal approach for this problem. The solution correctly prunes branches when the current character doesn't match (early termination) and uses backtracking to avoid extra memory allocation for visited tracking.

### Improvements
1. **Early return optimization**: Instead of checking each direction sequentially and immediately returning if found, use logical ORs to chain the recursive calls, which is more concise and avoids repeated conditional checks:
```cpp
bool ans = search(board, i+1, j, k+1, word) ||
           search(board, i-1, j, k+1, word) ||
           search(board, i, j+1, k+1, word) ||
           search(board, i, j-1, k+1, word);
```
2. **Parameter passing**: Pass `word` by const reference to avoid copying: `bool search(..., const string& word)`
3. **Boundary check order**: Check the visited status ('0') before boundary checks to avoid unnecessary boundary checks for already-visited cells.

### Why the percentile is low
Despite being theoretically optimal, the implementation has minor inefficiencies: sequential direction checks with immediate returns create more function call overhead compared to using logical ORs, which allows better branch prediction and potential short-circuiting. The solution also copies the `word` string repeatedly instead of using const reference.