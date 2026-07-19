---
problem: "Diameter of Binary Tree"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-19
---

# Analysis

### Verdict Summary
The solution uses a custom struct to store left/right depths and diameter at each node, recursively computing the diameter in O(n) time. The approach is correct but unnecessarily convoluted.

### Complexity
**Time:** O(n) — each node visited once.  
**Space:** O(h) recursion stack, worst-case O(n) for a skewed tree.

### vs. Optimal
This solution *is* asymptotically optimal (O(n) time, O(h) space). The known optimal approach is the same DFS that returns the maximum subtree depth while updating a global (or referenced) diameter variable. This implementation differs by storing three integers per node in a custom struct, adding overhead.

### Improvements
1. **Unnecessary struct:** The `Str` struct with `l`, `r`, and `d` is wasteful. Store only the current subtree’s depth; track diameter via a reference parameter or closure variable.
2. **Recursion base case redundant:** The `root->left==NULL && root->right==NULL` check is not needed — a null `root` case suffices.
3. **Unused zero-initialization:** `Str ans=Str();` then setting its fields is extra work. Directly compute and return a single integer for depth.

### Why the percentile is low
- Memory overhead from creating `Str` objects for every node.
- Extra arithmetic and struct copying per recursion step.
- Faster solutions use a single integer return (depth) and update an integer reference for the diameter, avoiding auxiliary data structures.