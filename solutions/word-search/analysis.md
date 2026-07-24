---
problem: "Word Search"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 9.6 MB
date: 2026-07-24
---

# Analysis

### Verdict summary
The code uses a DFS with backtracking approach, which is correct for this problem. However, the implementation is convoluted with redundant logic and incorrect recursive calls that render the solution inefficient and potentially incorrect for many cases.

### Complexity
**Time**: In the worst case, O(4^L * m * n) where L is the length of the word, but the implementation might prune poorly.
**Space**: O(L) for recursion depth, plus O(1) extra space aside from the recursion stack.

### vs. optimal
The optimal approach uses the same DFS backtracking but with clean, efficient pruning. This code differs by having redundant condition checks and incorrect recursive calls (e.g., `k` not incremented in some branches), which break the logic and cause unnecessary computations.

### Improvements
1. **Incorrect recursive calls**: In the branch where `val == word[k]`, the code should increment `k` for all subsequent searches. However, several calls (e.g., `search(board,i,j+1,k,word)`) use the same `k`, which is wrong.
2. **Redundant logic**: The `k==0` branch duplicates the search logic and is unnecessary. The main search should handle all cases uniformly.
3. **Early termination**: The code should return as soon as one path succeeds, but the current structure has multiple nested conditionals that obscure this.
4. **Pruning**: The code misses the basic pruning step: if the current character doesn't match `word[k]`, return immediately without exploring further.

### Why the percentile is low
Faster solutions use a cleaner DFS implementation: they check boundaries and visited status first, then check character match, and only then recurse with `k+1`. They also use a direction array for concise neighbor iteration. This code's redundant checks and incorrect recursion depth handling cause it to explore many invalid paths, leading to higher constant factors and potential errors.