---
problem: "Shift 2D Grid"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.5 MB
date: 2026-07-20
---

# Analysis

### Verdict summary
The submission uses a simulation approach that performs k shifts by repeatedly applying a single-shift function. While correct, this approach is inefficient for large k and does not leverage the cyclic nature of the problem.

### Complexity
- **Time complexity:** O(m * n * k), where m and n are the dimensions of the grid. Each `rotate` call does O(m * n) work, and it is called k times.
- **Space complexity:** O(m) for the `store` vector in each rotation, which is reused across calls.

### vs. optimal
The optimal approach computes the effective shift modulo the total number of elements (m * n), then maps each element directly to its final position without simulation. This runs in O(m * n) time and O(1) space (excluding output). The submission's simulation is suboptimal for large k.

### Improvements
1. **Avoid simulation:** Precompute k mod (m*n) and construct the result by calculating the flattened index shift.
2. **Eliminate temporary storage:** The current `rotate` uses a vector to store last column elements and does row-wise swaps, which is inefficient.
3. **Use direct indexing:** Instead of swapping elements in place, create a new grid and assign each element based on (i*n + j - k) mod (m*n).

### Why the percentile is low
Faster solutions compute the effective rotation in one pass by flattening the grid conceptually and using modulo arithmetic to determine each element's new position. This avoids the O(k) loop and reduces the constant factors by eliminating unnecessary swaps and temporary storage.