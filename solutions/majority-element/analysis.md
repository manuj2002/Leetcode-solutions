---
problem: "Majority Element"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-17
---

# Analysis

### Verdict summary
The submission uses **Boyer-Moore Majority Vote Algorithm**, which is correct for this problem. It efficiently finds the majority element in linear time and constant space without requiring sorting or a hash table.

### Complexity
- **Time complexity:** O(n) — A single pass through the array.
- **Space complexity:** O(1) — Only two integer variables (`item` and `cnt`) are used.

### vs. optimal
This **is the optimal approach** for the stated constraints and follow-up requirement (O(n) time, O(1) space). Boyer-Moore is the standard solution here, so no lecture needed.

### Improvements
1. **Logic clarity:** The condition checks could be restructured for better readability. The current branching is correct but slightly unconventional.
   - Instead of checking `item != e` twice, you can write it as:
     ```cpp
     if (cnt == 0) {
         item = e;
         cnt = 1;
     } else if (item == e) {
         cnt++;
     } else {
         cnt--;
     }
     ```
   - This eliminates redundant comparisons and aligns with the canonical Boyer-Moore implementation.

2. **Initialization:** `item = INT_MIN` is safe but unnecessary since the majority element always exists. You could initialize `item` to `nums[0]` and `cnt` to 1, then iterate from index 1. However, the current approach works correctly as-is.

### Why the percentile is low
Despite the optimal complexity, runtime percentiles on LeetCode can vary due to:
- **Micro-optimizations:** Other solutions may use a different loop structure (e.g., indexed loops instead of range-based) or avoid branching in a way that marginally improves performance on the judge’s test cases.
- **Luck of the draw:** The specific input distribution on the judge can cause small fluctuations. Since your solution is already O(n) and O(1), the differences are negligible and not worth over-optimizing.