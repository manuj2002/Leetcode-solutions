---
problem: "Path Existence Queries in a Graph II"
difficulty: unknown
verdict: Accepted
runtime: 1268 ms
memory: 466.6 MB
date: 2026-07-10
---

# Analysis

### Verdict summary
This solution attempts to use connectivity preprocessing and binary lifting to answer shortest path queries in a value-based graph. The core idea is valid—sorting nodes by their values and using binary lifting to find reachability in the sorted order—but the implementation has significant overhead that causes poor performance.

### Complexity
- Time complexity: O(n log n) for sorting + O(n) for two-pointer preprocessing + O(n log n) for binary lifting table construction + O(q log n) for query processing
- Space complexity: O(n log n) for the binary lifting table + O(n) for auxiliary arrays

### vs. optimal
The optimal approach for this problem involves:
1. Sorting nodes by value and computing reachability intervals in the sorted order
2. Using binary lifting to compute the maximum reachable index from each position
3. Answering queries by checking if nodes are in the same connected component, then using binary lifting to compute the minimal number of jumps between them

Your approach correctly implements this strategy, so the algorithm is conceptually optimal. However, the implementation contains inefficiencies that degrade performance.

### Improvements
1. **Reduce memory allocations**: The `vector<vector<int>> nodes` stores pairs of (value, index) but uses `vector<int>` for each pair, which is inefficient. Replace with `vector<pair<int, int>>` or two separate arrays.

2. **Eliminate redundant mapping**: The `nodeLocation` map is unnecessary. Since nodes are sorted by value, maintain an array `pos[n]` where `pos[i]` indicates the sorted position of node i.

3. **Simplify graph component tracking**: The current `graphSet` approach works but can be eliminated entirely. Connectivity can be determined by checking if the nodes are within reachable distance in the binary lifting structure.

4. **Optimize two-pointer logic**: The current two-pointer code is correct but could be written more efficiently without the `long long` cast.

### Why the percentile is low
The low percentile (beats 5.3%) is primarily due to:
- Excessive memory usage from nested vectors and maps
- Inefficient data structure choices (map instead of array for O(1) lookups)
- Memory locality issues from storing small vectors instead of flat arrays
- Overhead from unnecessary intermediate representations

Faster solutions use the same algorithmic approach but implement it with lower constant factors: using arrays instead of vectors-of-vectors, avoiding maps, and better memory layout.