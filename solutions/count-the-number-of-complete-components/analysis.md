---
problem: "Count the Number of Complete Components"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-11
---

# Analysis

### Verdict summary
The submitted code attempts to count connected components using BFS but fails to check whether each component is complete. The code incorrectly increments `ans` for every unvisited node without validating that the component is a clique. This is not the correct approach for the problem.

### Complexity
- **Time complexity**: O(n + e) where n is the number of nodes and e is the number of edges, due to the BFS traversal.  
- **Space complexity**: O(n + e) from storing the adjacency list and BFS queue.

### vs. optimal
The optimal approach for this problem involves:
1. Finding all connected components (using BFS/DFS/Union-Find).
2. For each component, verifying if it is complete by checking if the number of edges in the component equals `k*(k-1)/2` where `k` is the number of nodes in the component.

This code only performs step 1 and skips step 2 entirely. The complexity of the optimal solution is also O(n + e), but with an additional O(k²) check per component (which is acceptable since n ≤ 50).

### Improvements
1. **Missing completeness check**: After identifying a component, count its nodes (`k`) and edges. Compare the edge count to `k*(k-1)/2`.  
2. **Inefficient visited tracking**: Use a boolean `visited` array instead of `fre` (which is initialized to -1 but set to 1 without checking adjacency).  
3. **Unnecessary map**: `vector<vector<int>>` is preferable to `map<int, vector<int>>` for adjacency lists when nodes are numbered 0..n-1.  
4. **BFS logic error**: The condition `if(fre[e]!=-1)` should be `if(fre[e]==-1)` to enqueue unvisited nodes.

### Why the percentile is low
The code is ranked poorly because it returns the total number of connected components instead of complete components. Faster solutions correctly implement the completeness check, which is the core requirement. The BFS implementation here also has a logical error that prevents proper traversal.