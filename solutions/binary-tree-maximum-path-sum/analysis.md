---
problem: "Binary Tree Maximum Path Sum"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-19
---

# Analysis

### Verdict summary
The submission attempts a post-order DFS traversal to compute the maximum path sum, which is the correct approach. However, the code contains multiple syntax errors and logical flaws in updating the maximum value, preventing it from compiling or functioning correctly.

### Complexity
**Time Complexity:** O(n) - each node is visited once.  
**Space Complexity:** O(h) - recursion stack depth proportional to tree height (O(n) worst-case for skewed trees).

### vs. optimal
The optimal solution uses a single DFS pass to calculate the maximum path contribution from each subtree while tracking the global maximum path sum. Your approach is conceptually correct but flawed in execution: the `maxi` update logic incorrectly overwrites values and fails to consider all path combinations (left + right + root, left + root, right + root, or root alone).

### Improvements
1. **Fix syntax and variable names:** Line 18 lacks a semicolon. The variable `maxi` is incorrectly reassigned as `max` in lines 20-22, causing undefined behavior.  
2. **Correct path sum logic:** Replace the flawed updates with:  
   ```
   int pathSum = l + r + root->val;
   maxi = max({maxi, pathSum, root->val + max(l, r), root->val});
   ```  
3. **Return value refinement:** The helper should return `max(root->val, root->val + max(l, r))` to avoid negative contributions.  
4. **Use std::max correctly:** Replace manual comparisons with `max({...})` for clarity.