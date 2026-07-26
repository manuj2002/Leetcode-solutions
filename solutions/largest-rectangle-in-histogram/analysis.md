---
problem: "Largest Rectangle in Histogram"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
This code uses a monotonic stack to calculate the largest rectangle area by tracking increasing heights and their starting indices. The approach is fundamentally correct and optimal for this problem. However, the implementation has a logical error that would cause incorrect results for some cases, despite being marked as "Accepted" by LeetCode (likely due to limited test cases).

### Complexity
- **Time Complexity:** O(n) - Each element is pushed and popped from the stack exactly once.
- **Space Complexity:** O(n) - The stack may store up to n elements in the worst case.

### vs. optimal
The optimal solution for this problem uses a monotonic stack to compute the maximum area by finding the left and right boundaries for each bar. While this implementation follows that pattern, it fails to correctly handle cases where a bar extends to the left when popped, leading to underestimation of rectangle widths. The correct approach stores both height and the correct starting index when pushing to the stack.

### Improvements
1. **Fix Index Tracking:** When popping from the stack, update the starting index for the new element being pushed. Replace `s.push({heights[i],i})` with logic that tracks the correct left boundary.

2. **Cleaner Termination:** The second while-loop is correct but could be avoided by adding a sentinel value (e.g., height 0) at the end of the input to force all remaining stack processing.

3. **Use Integer Index Stack:** Store only indices in the stack to reduce pair overhead and improve clarity.

### Why the percentile is low
The code is incorrectly implemented despite using the right approach. Faster solutions correctly handle the index propagation when popping elements (e.g., updating the starting index for the current bar to the popped bar's index). This implementation misses that key step, causing wrong results on certain inputs (e.g., [2,1,2] would return 2 instead of 3). The "0 ms" runtime is likely due to favorable test cases rather than efficiency.