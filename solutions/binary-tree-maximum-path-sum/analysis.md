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
The submitted solution correctly implements a recursive post-order traversal to compute the maximum path sum by considering path combinations through each node. The approach is fundamentally correct and solves the problem optimally, though the implementation contains redundant calculations.

### Complexity
**Time Complexity:** O(n) where n is the number of nodes, as each node is visited exactly once.  
**Space Complexity:** O(h) where h is the tree height, due to the recursion stack (O(n) worst-case for a skewed tree).

### vs. optimal
This solution is optimal in its time and space complexity. The standard optimal approach uses a helper function that returns the maximum gain from a subtree (where "gain" is the maximum sum starting from the current node and going downward), while updating a global maximum that considers paths that "bend" through the current node (left + node + right). However, the current implementation is unnecessarily verbose in its maximum checks.

### Improvements
1. **Simplify the max calculations:** The four `maxi = max(...)` calls can be consolidated. The key insight is that the maximum path through the current node is `node->val + max(0, left) + max(0, right)`, and the return value for the parent is `node->val + max(0, max(left, right))`. The current code checks all combinations explicitly.
   
   Better:
   ```
   int leftGain = max(0, straightSum(root->left, maxi));
   int rightGain = max(0, straightSum(root->right, maxi));
   maxi = max(maxi, root->val + leftGain + rightGain);
   return root->val + max(leftGain, rightGain);
   ```

2. **Handle negative values more elegantly:** The current code handles negatives correctly but with redundant comparisons. The improved version above explicitly clamps gains at 0, which is cleaner.

3. **Function naming:** `straightSum` is misleading—it actually returns the maximum *straight* path sum starting from the current node going downward. A name like `maxGain` or `dfs` would be more standard.

### Why the percentile is low
Despite having optimal complexity, the percentile may be lower due to:
- **Redundant comparisons:** The four `max` operations per node are less efficient than the two needed in the cleaner version.
- **Compiler optimization:** The redundant checks might prevent some optimizations that the cleaner version allows.
- **Code clarity:** While not directly affecting runtime, cleaner code often correlates with better compiler optimization and cache performance. The top solutions use the more concise gain-clamping approach.