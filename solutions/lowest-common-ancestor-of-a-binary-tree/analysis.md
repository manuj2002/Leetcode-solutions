---
problem: "Lowest Common Ancestor of a Binary Tree"
difficulty: unknown
verdict: Accepted
runtime: 4 ms
memory: 8.3 MB
date: 2026-07-24
---

# Analysis

### Verdict summary
The submission implements a recursive DFS approach to find the LCA, which is correct in principle. However, the logic is unnecessarily complex with multiple condition checks that can be simplified while maintaining correctness.

### Complexity
- **Time Complexity:** O(n) - where n is the number of nodes, as it potentially visits every node in the worst case.
- **Space Complexity:** O(h) - where h is the tree height, due to the recursion stack. In the worst case (skewed tree), this becomes O(n).

### vs. optimal
The optimal approach for this problem is a simpler recursive DFS that returns the LCA directly by checking if the current root is one of the target nodes or if the targets are found in different subtrees. The optimal solution typically uses just three clean conditions:
1. If root is null, return null
2. If root is p or q, return root
3. Recursively search left and right subtrees
4. If both left and right return non-null, root is LCA
5. Otherwise, return the non-null subtree result

Your approach differs by having redundant condition checks comparing p1 and p2 directly with p and q, which is unnecessary since the recursive calls already handle the node identification.

### Improvements
1. **Simplify the logic:** Replace the complex condition checks with a cleaner approach:
```cpp
TreeNode* left = lowestCommonAncestor(root->left, p, q);
TreeNode* right = lowestCommonAncestor(root->right, p, q);
if (left && right) return root;
return left ? left : right;
```

2. **Remove redundant comparisons:** The current code has multiple conditions checking `(p1==p && p2==q)`, `(p1==q && p2==p)`, etc. These are redundant since the recursive structure already ensures correctness with simpler checks.

3. **Early null check consistency:** The initial null check is good, but the subsequent logic could be more streamlined.

### Why the percentile is low
The runtime percentile is low because the implementation contains unnecessary conditional branches and comparisons. Faster solutions use the cleaner recursive logic mentioned above, which reduces branching and allows for better optimization. The memory usage is reasonable since it uses the standard recursive approach, but the complexity of the condition checks adds overhead that affects performance.