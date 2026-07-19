---
problem: "Diameter of Binary Tree"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-19
---

# Analysis

### Verdict summary
The approach correctly uses DFS to compute the longest path through each node, but there's a syntax error (typo) that causes a compile error. The core idea is valid: calculate the depth of left and right subtrees and update the diameter at each node.

### Complexity
**Time:** O(n) — every node is visited exactly once.
**Space:** O(h) — recursion stack, where h is the tree height (worst-case O(n)).

### vs. optimal
This approach is optimal in time and space. The standard solution uses a similar DFS that returns the height of the subtree and updates a global (or passed-by-reference) diameter variable. However, the implementation here uses a custom `Str` object to return both height and diameter, which is slightly less efficient due to unnecessary struct copying.

### Improvements
1. **Fix syntax error**: Line 37 has a typo — `f` should be `if`.
2. **Avoid redundant struct copying**: Return just the height from the helper function and update the diameter via a reference parameter (or a global) to avoid constructing and copying `Str` objects for every node.
3. **Simplify logic**: The `Str` struct and null checks for children are overly complicated. The helper should simply return the subtree height (0 for null) and compute the diameter as `left_height + right_height`.
4. **Idiomatic C++**: Use `nullptr` instead of `NULL`. Prefer a single recursive function that updates an external diameter variable.

**Refactored code:**
```cpp
class Solution {
    int diameter;
    int dfs(TreeNode* root) {
        if (!root) return 0;
        int left = dfs(root->left);
        int right = dfs(root->right);
        diameter = max(diameter, left + right);
        return 1 + max(left, right);
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        diameter = 0;
        dfs(root);
        return diameter;
    }
};
```