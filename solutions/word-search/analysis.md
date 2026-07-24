---
problem: "Word Search"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 9 MB
date: 2026-07-24
---

# Analysis

### Verdict summary
The submission attempts a DFS with backtracking approach, which is correct for this problem. However, the implementation contains a critical bug in the recursion logic and index handling, making the solution incorrect despite being "accepted" (likely due to weak test cases). The function `search` is called with `(0,0,0)` regardless of the current cell `(i,j)` in the outer loops, so it only explores from the top-left corner repeatedly.

### Complexity
**Time Complexity**: Intended to be O(m * n * 4^L) where L is the length of `word`, but due to the bug, it only searches from (0,0) repeatedly, so worst-case is O(m * n * 4^L) but effectively only one starting point is used.  
**Space Complexity**: O(L) due to recursion depth (worst-case L), which is optimal for DFS.

### vs. optimal
The optimal approach is DFS + backtracking from every cell, with pruning (e.g., checking if the current board character matches the current word character before recursing). This solution has the right idea but fails to iterate over all starting points correctly. The known optimal time is O(m * n * 4^L), which is efficient given the constraints (small grid and word length).

### Improvements
1. **Critical Bug Fix**: In `exist`, the inner loops should call `search(board, i, j, 0, word)` instead of `search(board, 0, 0, 0, word)`. Currently, it only starts from (0,0) for every (i,j), which is incorrect.
2. **Pruning Logic**: The condition `if(board[i][j]==word[k]) return false;` is reversed. It should return false if they are NOT equal. Replace with `if(board[i][j] != word[k]) return false;`.
3. **Redundant Checks**: The DFS checks all four directions unconditionally. Instead, check if the current cell matches `word[k]` first, then mark and recurse.
4. **Code Cleanup**: Use a loop for directions (dx,dy arrays) to avoid repetitive code for each neighbor.
5. **Early Termination**: Return true immediately when k==word.size(), which is correct.

### Why the percentile is low
The solution has a severe bug (only searching from (0,0)) that causes it to fail on many test cases (e.g., if the word starts from another cell). However, if the test cases are weak and the word happens to start at (0,0), it might pass. Faster solutions correctly iterate over all starting points, use efficient pruning (return early if no match), and use direction loops for cleaner and more efficient recursion.