---
problem: "Word Search"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 9.7 MB
date: 2026-07-24
---

# Analysis

### Verdict summary
The code attempts a DFS backtracking approach but contains critical logic errors in handling the search state and base cases. The implementation incorrectly checks the current cell's value against the target character only after marking it visited, leading to incorrect pruning and missed paths.

### Complexity
**Time**: Worst-case O(m*n*4^L) where L is the word length, but the implementation has redundant calls due to flawed logic.  
**Space**: O(L) due to recursion depth, but O(m*n) worst-case if considering recursion stack (though L ≤ 15).

### vs. optimal
The optimal approach uses DFS backtracking with pruning: for each starting cell, recursively explore all 4 directions, backtracking by unmarking visited cells. This code has the same theoretical complexity but fails to correctly implement the search logic. The known optimal solution checks the current cell's value *before* marking it visited and only proceeds if it matches the current character in the word.

### Improvements
1. **Incorrect base case handling**: The code checks `board[i][j] != word[k]` *after* marking the cell as visited ('0'), which is backwards. The correct approach is to check if the current cell matches `word[k]` *before* marking it.
2. **Redundant recursion calls**: The code makes unnecessary recursive calls when `k==0` and when the current cell doesn't match, leading to duplicated and incorrect search paths.
3. **Inefficient backtracking**: The code attempts to backtrack immediately after any recursive call returns true, but the logic is convoluted and error-prone. The standard pattern is to explore all directions and return true if any succeed.
4. **Missing early termination**: The code does not prune when the current cell doesn't match `word[k]`, leading to wasted searches.

### Why the percentile is low
Faster solutions correctly implement DFS: they first validate the current cell matches `word[k]`, then mark it visited, recurse to all neighbors for `k+1`, unmark, and return the result. This avoids redundant calls and ensures correct pruning. The submitted code's flawed logic causes it to miss valid paths and make unnecessary searches, reducing efficiency despite the same asymptotic complexity.