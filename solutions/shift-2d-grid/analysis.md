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
The submission attempts a brute-force simulation by performing `k` separate shifts, each implemented by rotating elements through the grid. While correct for small constraints, this approach is inefficient for larger `k` and grid sizes.

### Complexity
**Time:** O(k * m * n) — Each shift operation traverses all m*n elements, and k shifts are performed.  
**Space:** O(1) — The rotation is done in-place with only constant extra variables.

### vs. optimal
The optimal approach computes the final position of each element in one pass using modular arithmetic. The grid is flattened into a 1D array of length `m*n`, and each element is moved to `(i * n + j + k) % (m*n)` in the new grid. This runs in O(m*n) time and O(m*n) space for the result (or O(1) extra space if done in-place with careful indexing). The submission's simulation is suboptimal for large `k`.

### Improvements
1. **Replace simulation with direct calculation:** Instead of shifting `k` times, compute the new grid in one pass. For each index `(i, j)`, the value should go to `(i + (j + k) / n) % m, (j + k) % n)`.
2. **Avoid unnecessary copies:** The `rotate` function returns a copy of the grid, but it modifies the input by reference. The return is redundant and incurs overhead.
3. **Fix the rotate function:** The function uses an uninitialized `k` (due to scoping) and has logic errors. It should save the last element, then shift row-by-row.

### Why the percentile is low
Faster solutions avoid simulation by directly computing the shifted positions using modulo operations. This reduces the time from O(k*m*n) to O(m*n), which is critical since `k` can be up to 100 and grid dimensions up to 50 (so k*m*n can be 250,000 vs. m*n=2,500). The submission's brute-force method is inefficient for large `k`.