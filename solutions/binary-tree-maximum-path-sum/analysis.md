---
problem: "Binary Tree Maximum Path Sum"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.1 MB
date: 2026-07-19
---

# Analysis

### Verdict summary
The solution uses a post-order DFS traversal to compute the maximum path sum, which is the correct approach. It calculates both the maximum path sum that can be extended upward and the global maximum path sum. The implementation is functionally correct but has code structure issues.

### Complexity
- **Time complexity:** O(n) - Each node is visited exactly once.
- **Space complexity:** O(h) - Due to recursion stack, where h is the height of the tree.

### vs. optimal
This IS the optimal approach for the problem. The standard solution uses DFS to compute:
1. Maximum sum of any path starting from current node and going downward (return value)
2. Global maximum path sum that may include the current node as the "root" of the path

### Improvements
1. **Function naming**: `straightSum` is misleading - `maxPathDown` or `dfs` would be clearer
2. **Redundant max operations**: The three `maxi = max(...)` calls can be consolidated into:
   ```cpp
   maxi = max({maxi, l + r + root->val, root->val, root->val + max(l, r)});
   ```
3. **Return logic**: The return statement can be simplified to:
   ```cpp
   return max(root->val, root->val + max(l, r));
   ```
   (this is already optimal)

### Why the percentile is low
Despite O(n) runtime, this implementation likely has slightly higher constant factors due to:
- Multiple separate max operations instead of a single max call
- Non-optimal compiler optimizations due to code structure
- The fastest solutions use the same algorithm but with more efficient implementations (e.g., using references consistently, better variable naming for compiler optimization)