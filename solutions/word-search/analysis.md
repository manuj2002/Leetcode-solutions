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
The approach attempts a backtracking DFS but is fundamentally flawed due to logical errors and incorrect implementation. The core idea of DFS with pruning is correct for this problem, but the code does not properly compare characters, handle the starting point, or explore all paths correctly.

### Complexity
This code would have exponential time complexity O(4^L * M * N) where L is the word length, M and N are board dimensions, but it contains critical bugs that prevent it from functioning. Space complexity would be O(L) for recursion depth if implemented correctly.

### vs. optimal
The optimal approach uses DFS backtracking: start DFS from every cell, marking visited cells, recursively exploring neighbors while matching word characters, then backtracking. Key differences:
1. Current code incorrectly handles the starting condition (k==0 separately)
2. Fails to properly check if current cell matches word[k] before exploring
3. Incorrectly passes k instead of k+1 in some DFS calls
4. Misses the fundamental pattern of "if match, then explore neighbors; else return false"

### Improvements
1. Fix syntax errors: Line 16 missing semicolon after `board[i][j]='0'`, line 54 missing semicolon after `return ans`, and multiple typos (`borad` vs `board`).

2. Simplify the logic flow: The current branching structure is confused. The correct pattern should be:
```cpp
if(board[i][j] != word[k]) return false;
if(k == word.size()-1) return true;
// Mark visited
// Explore 4 directions with k+1
// Unmark and return result
```

3. Remove redundant code: The separate handling for k==0 is unnecessary since the starting point should be handled by the main logic.

4. Fix neighbor exploration: All four directions should be explored with k+1 (not k) after a successful character match. The current code inconsistently mixes k and k+1 parameters.

5. Implement proper backtracking: The current code attempts to restore the board state in multiple places but does so incorrectly. The restoration should happen after all recursive calls complete.

6. The main function should iterate over all starting positions (i,j) rather than only starting at (0,0).