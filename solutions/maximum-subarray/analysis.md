---
problem: "Maximum Subarray"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-14
---

# Analysis

### Verdict summary
The code attempts to use a greedy approach but contains a critical off-by-one error by initializing with `nums[1]` instead of `nums[0]`, leading to a heap-buffer-overflow when the array has only one element. The logic is flawed even if corrected, as it doesn't properly reset the current sum when it becomes negative.

### Complexity
**Time:** O(n) — The code iterates through the array once.  
**Space:** O(1) — It uses constant extra space.

### vs. optimal
The optimal solution uses Kadane's algorithm, which maintains a running sum and resets it to the current element if the running sum becomes negative. Your code incorrectly adds the current element even when the running sum is negative (by only checking if `nums[i] > current_sum`), failing to reset properly. The correct Kadane's algorithm should initialize `current_sum` and `ans` to `nums[0]` and iterate from index 1, updating `current_sum = max(nums[i], current_sum + nums[i])`.

### Improvements
1. **Fix index error:** Start by initializing `current_sum` and `ans` to `nums[0]` and iterate from `i = 1` to `nums.size() - 1`.
2. **Correct the logic:** Replace the conditional with `current_sum = max(nums[i], current_sum + nums[i])` to properly reset the running sum when it becomes negative.
3. **Use standard library:** Use `std::max` directly for clarity and correctness.

Corrected code:
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int current_sum = nums[0], ans = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            current_sum = max(nums[i], current_sum + nums[i]);
            ans = max(ans, current_sum);
        }
        return ans;
    }
};
```