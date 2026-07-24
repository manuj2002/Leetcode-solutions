---
problem: "Word Search"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-24
---

# Analysis

### Verdict summary
The approach uses DFS backtracking but has critical logical errors and missing return statements. The core idea is correct (DFS with pruning via marking visited cells), but the implementation is flawed and does not compile.

### Complexity
**Time**: Without fixes, worst-case O(4^L * M*N) where L is word length, M and N are board dimensions. However, the current code has incorrect pruning and early termination logic.  
**Space**: O(L) for recursion depth, but O(M*N) if considering the modified board (though it's restored after each path).

### vs. optimal
The optimal approach is DFS backtracking with pruning by checking character matches before recursing. The current code attempts this but has severe issues:
1. It does not check if the current cell matches `word[k]` before exploring neighbors (except in a misplaced condition).
2. The logic for handling the first character (k=0) is fragmented and incorrect.
3. It misses the fundamental step: only recurse if the current cell matches the current character.

### Improvements
1. **Missing return in `exist`**: The function `exist` does not return the result of `search` (line 87). Add `return search(board,0,0,0,word);`.
2. **Incorrect condition structure**: The condition `if (board[i][j]!=word[k])` is misplaced. Instead, check at the start: if current cell doesn't match `word[k]`, return false. Also, mark the cell only after matching.
3. **Fix DFS logic**: The recursion should only proceed if the current character matches. The correct structure:
   - Check boundaries and visited.
   - If current char != word[k], return false.
   - If k == word.size()-1, return true.
   - Mark cell as visited.
   - Recurse in 4 directions with k+1.
   - Unmark cell and return if any path succeeded.
4. **Redundant recursions**: The code recurses with same `k` in some branches (e.g., line 50: `search(..., k, word)`), which is incorrect. Always increment `k` after matching.
5. **Inefficient pruning**: The current code does not prune early when the current character doesn't match. Move the match check to the top.

Revised core function:
```cpp
bool search(vector<vector<char>>& board, int i, int j, int k, string& word) {
    if (k == word.size()) return true;
    if (i < 0 || j < 0 || i >= board.size() || j >= board[0].size() || board[i][j] != word[k]) 
        return false;
    char tmp = board[i][j];
    board[i][j] = '0';
    bool found = search(board, i+1, j, k+1, word) ||
                 search(board, i-1, j, k+1, word) ||
                 search(board, i, j+1, k+1, word) ||
                 search(board, i, j-1, k+1, word);
    board[i][j] = tmp;
    return found;
}
```
And in `exist`, iterate over all starting cells.