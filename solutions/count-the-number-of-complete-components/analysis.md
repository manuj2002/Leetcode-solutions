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
The code uses BFS to find connected components and checks if each component is complete by verifying if every vertex has degree equal to component size minus one. This is the correct overall approach to solve the problem.

### Complexity
- **Time:** O(n + m) where m is the number of edges, but the adjacency map uses vectors and scanning all vertices for degree checking does O(n) per BFS run, leading to O(n²) worst case due to repeated scanning of vertices.
- **Space:** O(n + m) for the adjacency map and BFS queues. The `fre` array takes O(n).

### vs. optimal
The known optimal approach is to compute component vertices via DFS/BFS or union-find, count vertices and edges within each component, then check if `edge_count == (vertices * (vertices - 1) / 2)` for completeness. This avoids repeatedly scanning adjacency lists per vertex within a component, achieving O(n + m) time. Your approach does O(n + m) for traversal but then does a linear scan over each component’s vertices, each checking adjacency size, which is okay but less direct than counting edges.

### Improvements
1. **Edge counting:** Instead of checking `nx-1 == adj[qx.front()].size()` for each vertex, count actual unique edges per component during BFS/DFS and compare to `nx*(nx-1)/2` for completeness. This avoids issues if edges are directed incorrectly in adjacency logic and is cleaner.
2. **Data structures:** Use `vector<unordered_set<int>>` or `vector<vector<int>>` with fixed size `n` instead of `map<int,vector<int>>` for O(1) lookups and better cache locality.
3. **BFS iteration:** The BFS marks `fre[e]=1` when pushing but marks the start vertex late; better to mark `fre[i]=1` before BFS to avoid duplicate pushes.
4. **Queue duplication:** Storing vertices in `qx` is redundant; store vertices in a `vector<int>` during BFS for simpler iteration.

### Why the percentile is low
Faster solutions use union-find to group vertices and a separate `edges_in_component` count; they iterate over `edges` once to union vertices and increment edge count per component, then check completeness with one calculation per component. This avoids building full adjacency lists and redundant BFS degree checks, reducing constant factors and memory overhead.