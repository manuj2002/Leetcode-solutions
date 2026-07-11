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
The approach finds connected components via BFS, then checks if each component is a complete graph by comparing every vertex’s degree to `component_size – 1`. This works but is inefficient in structure and contains redundant output and data structures, though the core idea is correct.

### Complexity  
**Time:** O(n + m) where m = number of edges, due to BFS traversal and degree checks per component.  
**Space:** O(n + m) for adjacency list, plus O(n) for queues and frequency array.

### vs. optimal  
The optimal approach is BFS/DFS or union-find to find components, then verify that each component of size `k` has exactly `k*(k-1)/2` edges. This avoids per-vertex degree checks and uses edge counts. Your method does per-vertex degree checks which is fine asymptotically, but the edge-count method is simpler and less error-prone.

### Improvements  
1. **Remove unnecessary `cout` statements** — lines like `cout<<...` slow down runtime and serve no purpose.  
2. **Replace `fre` array with a boolean visited array** — using `-1`/`1` is misleading; `vector<bool>` would be clearer.  
3. **Use adjacency adjacency list more idiomatically** — `map<int,vector<int>>` is overkill; `vector<vector<int>> adj(n)` is fixed-size and faster.  
4. **Merge BFS and degree checking** — compute component vertices and their degrees in a single pass, avoiding a second queue `qx` (just store vertices in a vector during BFS).  
5. **Simplify complete-check logic** — instead of checking `nx-1 != adj[...].size()`, count edges in component and compare to `nx*(nx-1)/2`.

### Why the percentile is low  
The solution unnecessarily uses `map` instead of `vector` for adjacency, includes debug output, and recomputes degree checks per vertex separately. Faster solutions precompute edges per component via union-find or adjacency matrix, then verify clique condition directly without per-vertex loops inside the BFS.