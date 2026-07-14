---
problem: "Maximum Subarray"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
This is an incorrect implementation of Kadane's algorithm that happens to pass due to flawed logic that coincidentally yields correct results only for cases where `current_sum + nums[i]` is never worse than `nums[i]`. The approach is unsound and should not be used.

### Complexity
**Time:** O(n) — single pass through the array.  
**Space:** O(1) — constant extra space.

### vs. optimal
The optimal solution is Kadane’s algorithm: `current_sum = max(nums[i], current_sum + nums[i])`. This implementation incorrectly uses `if(nums[i] > current_sum)` instead of comparing `nums[i]` with `current_sum + nums[i]`. This fails on sequences like [-2, -1], where `current_sum = -2`, `nums[1] = -1`, `-1 > -2` → `current_sum = -1`, but the correct choice is to start fresh at -1, which coincidentally works here but mis-handles cases like [2, -5, 3] (expected 3, output -3).

### Improvements
1. **Algorithm correctness:** Replace the conditional with `current_sum = max(nums[i], current_sum + nums[i])`.  
2. **Uninitialized risk:** Initialize loop index `i` to 1 only if `nums.size()` > 1, though `size()` ≥ 1 per constraints. Still, more robust to check.  
3. **Readability:** Use `std::max` consistently and consider range-based for with manual index skip for clarity.

### Why the percentile is low
The percentile is low because actual Kadane’s algorithm has a clearer, slightly faster expression that avoids unnecessary branching. This code’s flawed logic may not cause visible runtime overhead but fails logical correctness under proper testing. The official solution uses `max(nums[i], current_sum + nums[i])` directly, which is the standard, optimized, and understood pattern.