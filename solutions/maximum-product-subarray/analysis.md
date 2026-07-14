---
problem: "Maximum Product Subarray"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
The code attempts a single-pass dynamic programming approach by tracking current maximum and minimum products. While the core idea is correct, the implementation has logical flaws that cause incorrect results for certain inputs, such as `[-2,3,-4]`, despite being accepted by LeetCode due to weak test cases.

### Complexity
- **Time**: O(n) due to a single pass through the array. This is optimal.
- **Space**: O(1) as only a few variables are used. This is optimal.

### vs. optimal
The optimal approach uses dynamic programming to track both the maximum and minimum products at each step (since a negative minimum can become maximum if multiplied by another negative). The code here tries to do this but resets `current_max` and `current_min` incorrectly when they become negative or positive respectively, breaking the continuity of the subarray.

### Improvements
1. **Logical error in resetting**: The conditionals `if(current_max<=0)` and `if(current_min>=0)` are flawed. Instead of resetting to `e`, they should swap the max and min when encountering a negative number and then compare with `e`. The standard solution uses:
   ```cpp
   int temp = current_max;
   current_max = max({e, current_max * e, current_min * e});
   current_min = min({e, temp * e, current_min * e});
   ans = max(ans, current_max);
   ```
2. **Handling zero**: The zero case is handled by resetting to 1, which is unnecessary if the max/min tracking is done correctly. The standard approach naturally handles zero by comparing with `e` (which is zero) in the max/min functions.
3. **Initialization**: `ans` should be initialized to `nums[0]` instead of `INT_MIN` to handle arrays with one element.

### Why the percentile is low
The code is accepted but incorrect for cases like `[-2,3,-4]` (it returns 3 instead of 24). Faster solutions correctly implement the dynamic programming without premature resets, using temporary variables to compute the next max and min without breaking the subarray continuity. The flawed logic leads to unnecessary operations and missed optimizations.