---
problem: "Word Search"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-24
---

# Analysis

### Verdict summary
The approach uses backtracking with DFS to explore all possible paths starting from each cell that matches the first character of the word. This is correct and efficient for the constraints, leveraging pruning by early termination when mismatches occur.

### Complexity
**Time:** O(m * n * 4^L), where m and n are the board dimensions and L is the length of the word. Each DFS call branches into 4 directions, but pruning stops exploration when the path doesn't match.
**Space:** O(L) for the recursion stack depth, as no additional data structures are used beyond the modified board.

### vs. optimal
This is the optimal approach for this problem. The backtracking DFS with pruning (by marking visited cells and checking bounds/character matches) is standard and matches the expected solution. The complexity is unavoidable in the worst case.

### Improvements
1. **Use a reference for `word`**: Pass `word` by const reference in the `search` function to avoid copying the string in each recursive call.
2. **Combine direction checks**: Use arrays for direction offsets (dx, dy) and loop over them to avoid repetitive code and reduce branching.
3. **Early return optimization**: Check the current character before proceeding to recursive calls to avoid unnecessary stack frames.

### Why the percentile is low
Despite being optimal, the percentile might be lower due to:
- **Inefficient direction handling**: The repetitive code for each direction (instead of a loop) may cause minor overhead.
- **Lack of additional pruning**: Some solutions pre-check if the board has enough characters or use a frequency table to skip impossible cases, though this isn't necessary for the given constraints.
- **Board modification**: Using the board to mark visited cells is efficient, but some solutions use a separate visited matrix, which might be faster in languages with cheaper allocation (though not critical here).