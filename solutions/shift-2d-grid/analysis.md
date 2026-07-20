---
problem: "Shift 2D Grid"
difficulty: unknown
verdict: Accepted
runtime: 39 ms
memory: 48.6 MB
date: 2026-07-20
---

# Analysis

### Verdict summary
The submission uses a simulation approach that performs k shifts by rotating the grid one step at a time. While correct, this approach is inefficient for large k due to its high time complexity.

### Complexity
- **Time complexity:** O(k * m * n), where m and n are the grid dimensions. Each `rotate` call does O(m * n) work (due to inner loops and swaps), and this is called k times.
- **Space complexity:** O(m) for the `store` vector, which is acceptable but not optimal.

### vs. optimal
The optimal approach flattens the grid into a 1D array of size m*n, computes the effective shift modulo m*n, and then reconstructs the grid. This runs in O(m*n) time and O(m*n) space. The submission's simulation is suboptimal because it performs k shifts individually, which is inefficient when k is large (up to 100, and m*n up to 2500).

### Improvements
1. **Avoid simulation for large k:** Instead of simulating k shifts, compute the effective shift as `k % (m*n)`. If zero, return the original grid; otherwise, flatten the grid, rotate the flattened array by `k % (m*n)` positions, and reshape.
2. **Eliminate redundant operations:** The current `rotate` function uses a temporary vector and nested loops. This can be replaced with a single pass over a flattened array.
3. **Use direct indexing:** After flattening, the new grid can be constructed by mapping each index (i, j) to the flattened index `(i*n + j - k) % (m*n)` (adjusted for negative modulo).

### Why the percentile is low
The runtime percentile is low because the solution simulates each shift individually, leading to unnecessary operations when k is large. Faster solutions compute the effective shift modulo the grid size and directly construct the result without iterative steps, reducing the time complexity from O(k*m*n) to O(m*n).