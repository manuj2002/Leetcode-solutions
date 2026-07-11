---
problem: "Count the Number of Complete Components"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.6 MB
date: 2026-07-11
---

# Analysis

### Verdict summary
The submission attempts to count connected components using BFS but fails to address the core requirement of checking if each component is complete. It returns the number of connected components rather than complete components, which is incorrect for the problem.

### Complexity
**Time complexity:** O(n + e) where e is the number of edges, due to BFS traversal.  
**Space complexity:** O(n + e) for adjacency list and visited array.

### vs. optimal
The optimal approach requires identifying each connected component and verifying if it forms a complete graph. For a component with k vertices, this requires checking if it has exactly k*(k-1)/2 edges. The submission only counts connected components without checking completeness, making it incorrect. The correct solution typically uses DFS/BFS to find components, then validates edge counts.

### Improvements
1. **Missing completeness check:** The algorithm needs to track vertices in each component and verify that the number of edges within the component matches k*(k-1)/2.
2. **Inefficient edge counting:** Instead of using a map for adjacency, store edges in a set or use an adjacency matrix to quickly check edge existence between vertices in the same component.
3. **Unnecessary map usage:** `map<int, vector<int>>` can be replaced with `vector<vector<int>>` since vertices are numbered 0 to n-1, improving cache performance.

### Why the percentile is low
The code is incorrect—it fails basic test cases (e.g., Example 2 returns 1 instead of 3). Faster solutions correctly implement completeness checks by counting edges per component, often using union-find with edge counting or component traversal with adjacency matrices. The submission misses the problem's core requirement entirely.