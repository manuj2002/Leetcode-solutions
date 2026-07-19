---
problem: "Binary Tree Maximum Path Sum"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-19
---

# Analysis

## Verdict summary
The submission uses a correct post-order DFS approach to compute the maximum path sum. It calculates the maximum path that can be extended upward from each node while simultaneously tracking the global maximum. The approach is fundamentally correct and solves the problem optimally.

## Complexity
- **Time complexity:** O(n) where n is the number of nodes. Each node is visited exactly once.
- **Space complexity:** O(h) where h is the tree height, due to recursion stack. In worst-case (skewed tree), this becomes O(n).

## vs. optimal
This solution **is optimal** in terms of time and space complexity. The standard optimal approach for this problem is exactly what's implemented: a DFS that computes the maximum path sum passing through each node while returning the maximum sum that can be extended upward. No significant differences from the optimal approach.

## Improvements
1. **Function naming**: `straightSum` is misleading - it actually returns the maximum sum of a path starting from the current node and going downward. Better names would be `maxPathDown` or `dfs`.
2. **Redundant max checks**: The three `maxi = max(...)` calls can be simplified:
   ```cpp
   maxi = max({maxi, l + r + root->val, root->val, root->val + max(l, r)});
   ```
3. **Return value optimization**: The return statement `return root->val + max(l, r)` should clamp negative values to zero for better clarity:
   ```cpp
   return max(root->val, root->val + max(l, r));
   ```
4. **Handle negative sums properly**: When `l` or `r` are negative, they should be treated as 0 since we can choose not to include them.

## Why the percentile is low
Despite being O(n) complexity, the percentile might be lower due to:
- **Unnecessary comparisons**: The code performs more max operations than needed. An optimized version would compute `max(0, l)` and `max(0, r)` first, then calculate path sums.
- **Recursive overhead**: While minimal, some solutions might use iterative DFS to avoid recursion overhead.
- **Code clarity**: The logic, while correct, is less readable than the standard solution that explicitly handles negative contributions by taking `max(0, left/right)`.