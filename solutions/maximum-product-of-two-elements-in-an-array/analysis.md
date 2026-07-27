---
problem: "Maximum Product of Two Elements in an Array"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-27
---

# Analysis

### Verdict summary
The approach sorts the array and multiplies the two largest values (minus one). This is correct since the maximum product of two elements occurs when both factors are maximized. The solution is straightforward and meets the problem requirements.

### Complexity
**Time complexity:** O(n log n) due to sorting.  
**Space complexity:** O(log n) for the sorting algorithm's stack space (or O(1) if in-place sorting like heapsort is used, though `std::sort` typically uses O(log n) recursion space).

### vs. optimal
The optimal solution runs in O(n) time by scanning the array once to find the two largest elements without sorting. The current approach is asymptotically slower due to sorting, but still acceptable given the small constraints (n ≤ 500).

### Improvements
1. **Replace sorting with a linear scan:**  
   Find the two largest values with a single pass:
   ```cpp
   int max1 = 0, max2 = 0;
   for (int x : nums) {
       if (x > max1) {
           max2 = max1;
           max1 = x;
       } else if (x > max2) {
           max2 = x;
       }
   }
   return (max1 - 1) * (max2 - 1);
   ```
2. **Minor readability:** The current code has unnecessary parentheses around the return expression.

### Why the percentile is low
Faster solutions avoid sorting entirely and use a single O(n) pass to track the top two values. Sorting incurs logarithmic overhead, which is measurable even for small n, and is unnecessary for finding just two elements. The O(n) approach is both asymptotically optimal and faster in practice.