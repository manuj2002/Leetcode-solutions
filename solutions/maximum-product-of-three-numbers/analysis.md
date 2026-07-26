---
problem: "Maximum Product of Three Numbers"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-26
---

# Analysis

### Verdict summary
The code attempts to sort the array and then check the product of the three largest numbers versus the product of the two smallest and the largest number, which is the correct approach. However, it contains a syntax error that prevents compilation.

### Complexity
- **Time complexity:** O(n log n) due to sorting.
- **Space complexity:** O(log n) for the sorting algorithm's stack space (or O(1) if heap sort is used, but typically quicksort is O(log n) stack space).

### vs. optimal
The optimal approach for this problem is to find the three largest and two smallest numbers in a single pass, achieving O(n) time and O(1) space. The submitted code uses sorting, which is suboptimal in time complexity.

### Improvements
1. **Fix the compile error:** On line 6, `num[n-2]` should be `nums[n-2]`.
2. **Optimize for linear time:** Instead of sorting, scan the array once to find the three largest and two smallest values, then compute the maximum product from `max1 * max2 * max3` and `min1 * min2 * max1`.
3. **Consider edge cases explicitly:** The current logic handles the case with negative numbers correctly via the second product, but a linear scan would be more efficient and avoid sorting overhead.