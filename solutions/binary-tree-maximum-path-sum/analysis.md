---
problem: "Binary Tree Maximum Path Sum"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-19
---

# Analysis

### Verdict summary
The code implements a correct post-order DFS approach to compute the maximum path sum by tracking both the maximum path through each node and the maximum single-branch sum that can be extended upward. This is the right approach for the problem.

### Complexity
**Time complexity:** O(n) - Every node is visited exactly once.  
**Space complexity:** O(h) - Due to recursion stack height, which is O(n) worst-case for skewed trees, O(log n) for balanced trees.

### vs. optimal
This IS the optimal approach. The solution correctly computes two values at each node: the maximum path sum that includes the current node (which may involve both children for the global max), and the maximum single-branch sum that can be passed to the parent (using only one child). The time and space complexities are optimal.

### Improvements
1. **Function naming:** `straightSum` is misleading - it actually returns the maximum branch sum, not a "straight" path. Better names like `dfs` or `maxBranchSum` would improve readability.
2. **Return value handling:** The three `maxi` updates could be consolidated:
   ```cpp
   int branch_sum = root->val + max(l, r);
   maxi = max({maxi, l + r + root->val, root->val, branch_sum});
   return max(root->val, branch_sum); // Actually branch_sum already includes root->val
   ```
3. **Edge case optimization:** Check for null children before recursive calls to avoid function call overhead (though compiler may optimize this).
4. **Integer safety:** Since node values can be negative, the return value should be `max(root->val, root->val + max(l, r))` to ensure we don't return a sum worse than just the node itself.

### Why the percentile is low
Despite being algorithmically optimal, the low percentile likely stems from:
- **Multiple max operations:** The three separate `max` calls could be combined into one, reducing instruction count.
- **Function call overhead:** The recursion is necessary, but some solutions use iterative DFS with explicit stacks which can be faster in practice.
- **Memory locality:** The recursive solution has worse cache performance compared to iterative approaches that use a contiguous stack array.