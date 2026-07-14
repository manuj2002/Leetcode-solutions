---
problem: "Maximum Product Subarray"
difficulty: unknown
verdict: Wrong Answer
runtime: N/A
memory: N/A
date: 2026-07-14
---

# Analysis

### Verdict summary
The approach attempts dynamic programming by tracking current maximum and minimum products, but the reset conditions are flawed. It fails on the test case [0,2] because it incorrectly resets `current_max` and `current_min` in a way that breaks the continuity of the subarray product calculation.

### Complexity
**Time:** O(n) — single pass through the array.  
**Space:** O(1) — constant extra space for variables.

### vs. optimal
The known optimal approach uses dynamic programming to maintain both the current maximum and minimum products at each step (to handle negatives) and updates them by considering the current number alone or multiplied by the previous max/min. Your code attempts this but has incorrect reset logic: instead of resetting when the product is non-positive, the optimal method should swap max and min when encountering a negative and always compare with the current element to reset if beneficial.

### Improvements
1. **Incorrect reset conditions**: The reset logic `if (current_max <= 0) current_max = e` and `if (current_min > 0) current_min = e` is flawed. Instead, before updating, store the previous max and min, then compute new max and min as `max(e, e * prev_max, e * prev_min)` and `min(e, e * prev_max, e * prev_min)` respectively.
2. **Missing swap for negatives**: When `e` is negative, the roles of max and min should be swapped because multiplying by a negative inverts the values.
3. **Initialization issue**: `ans` should be initialized to `INT_MIN`, but you must also consider cases where the array has a single element or all negatives. The current code might reset incorrectly before updating `ans`.

**Fixed code:**
```cpp
int maxProduct(vector<int>& nums) {
    int current_max = nums[0], current_min = nums[0], ans = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        int e = nums[i];
        if (e < 0) swap(current_max, current_min);
        current_max = max(e, current_max * e);
        current_min = min(e, current_min * e);
        ans = max(ans, current_max);
    }
    return ans;
}
```