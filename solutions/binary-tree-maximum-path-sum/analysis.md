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
The submission attempts a recursive DFS approach to compute the maximum path sum, which is the correct high-level strategy. However, the implementation contains multiple critical errors, including a missing semicolon, incorrect variable names, and wrong parameter passing, making it uncompilable.

### Complexity
- **Time Complexity:** O(n) — each node is visited exactly once.
- **Space Complexity:** O(h) — due to recursion stack, where h is the tree height (worst-case O(n) for a skewed tree).

### vs. optimal
The optimal solution uses a single DFS traversal to compute two values at each node: the maximum path sum that can be extended upward (to be used by the parent) and the global maximum path sum (which may include the current node as the "root" of a path). This submission correctly recognizes the need to track a global maximum and compute local contributions, but the implementation is flawed. The optimal approach would handle four cases at each node: the node alone, node + left branch, node + right branch, and node + both branches as a complete path.

### Improvements
1. **Fix function signature and parameters:** The recursive function `straightSum` is called with only one argument on lines 16 and 17, but its definition requires two parameters (`root` and `maxi`). Pass the `maxi` reference in all recursive calls.
2. **Correct variable names:** Lines 19–22 use undefined variables `max` and `maxi` incorrectly. Use `maxi` consistently for the global maximum and fix the calculations.
3. **Add missing semicolon:** Insert a semicolon after `int r=straightSum(root->right)` on line 18.
4. **Logical error in calculations:** The current max-updating logic is jumbled. The correct logic should be:
   ```cpp
   int local_max = max(root->val, root->val + max(l, r));
   maxi = max(maxi, max(local_max, l + r + root->val));
   return local_max;
   ```