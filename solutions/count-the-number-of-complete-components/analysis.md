---
problem: "Count the Number of Complete Components"
difficulty: unknown
verdict: Accepted
runtime: 2 ms
memory: 8.3 MB
date: 2026-07-11
---

# Analysis

### Verdict summary
The approach uses BFS to traverse each connected component and checks if the component is complete by verifying that every node has degree equal to (component size - 1). This is correct and efficient for the constraints (n ≤ 50).

### Complexity
- **Time complexity:** O(V + E) for BFS traversal, and O(V) per component to check degrees, leading to O(V²) worst-case.
- **Space complexity:** O(V + E) for the adjacency list and visited array.

### vs. optimal
This solution is optimal for the given constraints. The known optimal approach is to compute connected components and verify completeness by comparing the number of edges in the component to the required number for a complete graph (which is k*(k-1)/2 for a k-node component). Alternatively, checking that every node has degree (k-1) is equivalent and efficient for small n.

### Improvements
1. **Avoid redundant data structures:** The `qx` queue duplicates the nodes already stored in the component. Instead, store nodes in a vector during BFS to avoid a second queue.
2. **Precompute component nodes:** Collect nodes in a list during BFS, then iterate over that list to check degrees.
3. **Use adjacency list efficiently:** The current code builds an adjacency list correctly, but the check loop could be optimized by precomputing the component set.
4. **Avoid map for adjacency:** Use a vector<vector<int>> of size n instead of a map for better performance and clarity.
5. **Remove debug output:** The `cout` statements should be removed in production code.

### Why the percentile is low
The runtime percentile is low due to the use of a map for the adjacency list (which has logarithmic access) instead of a vector of vectors, and the redundant queue `qx` which adds unnecessary operations. Faster solutions use union-find or DFS with precomputed component lists and vector-based adjacency lists for constant-time access.