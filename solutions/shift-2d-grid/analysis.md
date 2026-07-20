---
problem: "Shift 2D Grid"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.6 MB
date: 2026-07-20
---

# Analysis

### Verdict summary
The approach uses a simulation that performs k shifts by rotating the grid one step at a time. While correct, this is inefficient for large k since each shift requires O(m*n) operations, leading to O(k*m*n) total time.

### Complexity
Time: O(k * m * n) — For each of k shifts, the code traverses most elements in the m x n grid.
Space: O(m) — The `store` vector holds m elements for the last column.

### vs. optimal
The optimal approach flattens the grid into a 1D array of size m*n, computes the effective shift modulo m*n, and reconstructs the grid. This runs in O(m*n) time and O(m*n) space. Your simulation is suboptimal for large k (up to 100) and grid sizes (up to 50x50=2500 elements), as k*m*n could be 250,000 operations vs. 2500 for the optimal.

### Improvements
1. Replace simulation with modulo indexing: Flatten the grid, compute start index = (m*n - k) % (m*n), and map each position (i, j) to index (start + i*n + j) % (m*n) in the flattened array.
2. Avoid O(k) outer loop: The current loop runs k times, which is inefficient when k is large.
3. Eliminate redundant swaps: The inner loop uses a swap chain that is less efficient than direct assignment.

### Why the percentile is low
Faster solutions use the modulo indexing method to compute the shifted grid in a single pass without simulating each shift. Your solution performs up to 100 shifts, each involving O(2500) operations, while the optimal does exactly 2500 operations total.