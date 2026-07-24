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
Your approach attempts backtracking with DFS but is fundamentally broken due to a typo (`borad` vs `board`) and multiple logic errors. The correct solution should start from every cell and recursively explore neighbors using careful backtracking; you are not correctly implementing that flow.

### Complexity
If the code compiled, it would be **O(m * n * 4^L)** time in worst-case where L is word length, because it explores up to 4 directions without early pruning for mismatched characters. Space complexity would be **O(L)** for recursion depth, plus O(1) extra aside from marking visited cells in-place.

### vs. optimal
The optimal solution is also backtracking + DFS but is properly structured:  
1. Loop through all starting cells `(i, j)`.  
2. In DFS, first check bounds, visited status, and character match at current `board[i][j]` vs `word[k]`.  
3. If match and `k == L-1`, return true; else mark visited, recurse four directions with `k+1`, unmark, return success if any direction works.  
This has the same time complexity in worst case, but early mismatching and bounds checks prune branches immediately—your code fails to check character match before recursing in some branches, causing wasted exploration.

### Improvements
1. **Fix typo and compilation errors:** Change `borad` to `board` on every occurrence; add missing semicolons after `return ans` in two places.
2. **Restructure DFS:** Your conditional `if(board[i][j]!=word[k])` is misplaced—that check belongs at the start before exploring neighbors (and when false, you should not explore). After marking, you should only propagate `k+1` if current character matches `word[k]`.
3. **Starting position logic:** In `exist`, you must iterate over all `(i,j)` and call `search` from each—currently you only call `search(board,0,0,0,word)`.
4. **Remove redundant code:** The branch `if(k==0)` is unnecessary, and your neighbor recursions sometimes pass `k` (not `k+1`) incorrectly; standard DFS increments `k` only after matching current cell.