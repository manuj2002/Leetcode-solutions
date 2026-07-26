---
problem: "Maximum Product of Three Numbers"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
The submission uses sorting to find the three largest numbers and returns their product. While this works for many cases, it fails to consider negative numbers that could yield a larger product when multiplied with positive numbers.

### Complexity
Time: O(n log n) due to the sorting operation. Space: O(1) if the sort is in-place (as it is here), but note that the algorithm may use O(log n) stack space for the sort.

### vs. optimal
The optimal approach for this problem is to find the two smallest and three largest numbers in a single pass without sorting. The maximum product is either the product of the three largest numbers or the product of the two smallest (which could be negative) and the largest (if they yield a positive product). The optimal solution runs in O(n) time and O(1) space.

### Improvements
1. Replace the O(n log n) sort with a linear scan to track the two smallest and three largest values.
2. Instead of sorting the entire array, which is unnecessary, maintain variables for min1, min2, max1, max2, max3.
3. The current solution fails for cases with negative numbers (e.g., [-10, -10, 1, 2] should return 200, but this code returns 2).

### Why the percentile is low
The percentile is low because the solution uses sorting, which is asymptotically slower than the optimal linear approach. Faster solutions avoid sorting and instead scan the array once, updating the necessary min and max values, which is O(n) and more efficient for large inputs.