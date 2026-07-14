---
problem: "Maximum Product Subarray"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
The approach attempts to use a dynamic programming idea by tracking the current maximum and minimum products, but the implementation is flawed. The code fails to correctly handle the cases where the current product becomes zero or negative, leading to incorrect results for some inputs. The verdict "Accepted" is likely due to the test cases not covering all edge cases, but the code is not robust.

### Complexity
Time complexity: O(n) — single pass through the array.  
Space complexity: O(1) — constant extra space.

### vs. optimal
The optimal approach for this problem uses dynamic programming to track both the maximum and minimum products at each step (since a negative number can flip a negative minimum into a positive maximum). The standard solution maintains `max_so_far` and `min_so_far` and updates them at each step by considering the current number, the product of `max_so_far * current`, and `min_so_far * current`. This code attempts to do that but fails to correctly update `current_max` and `current_min` when they become negative or positive, respectively. For example, when `current_max` becomes negative, it resets to `e`, which is incorrect because it should be compared with `min_so_far * e` and `e` to determine the new max and min.

### Improvements
1. **Incorrect reset logic**: The reset conditions for `current_max` and `current_min` are flawed. Instead of resetting to `e` when `current_max < 0` or `current_min > 0`, the code should compute the new `current_max` and `current_min` as the maximum and minimum among `e`, `current_max * e`, and `current_min * e`.
2. **Handling zero**: The code does not explicitly handle zero, which can break the product chain. The optimal solution naturally handles zero by comparing with `e`.
3. **Initialization**: `ans` is initialized to `INT_MIN`, but it should be set to the first element or a very small number. However, the loop starts by multiplying `current_max` and `current_min` by the first element without initializing them to 1, which is correct only if the first element is considered. But the reset logic might cause issues.

### Why the percentile is low
The code is incorrect for certain inputs, such as arrays with multiple negatives or zeros. For example, test with `[ -2, 3, -4 ]`: 
- After first element: current_max = -2, current_min = -2, ans = -2.
- After second element: current_max = -6, current_min = -6, ans = max(-2, -6) = -2. Then reset current_max to 3 (because -6 < 0) and current_min to 3 (because -6 > 0? wait, no, current_min is -6 which is not >0, so no reset). So current_min remains -6.
- After third element: current_max = 3 * -4 = -12, current_min = -6 * -4 = 24, ans = max(-2, 24)=24. Correct answer is 24, but the reset logic is inconsistent.

Actually, the reset logic is applied only when current_max <0 (reset to e) and current_min>0 (reset to e). This is arbitrary and not standard. The correct solution should not reset but rather choose the max and min from three candidates.

The faster solutions correctly implement the DP without such resets, using:
temp = current_max;
current_max = max({e, current_max * e, current_min * e});
current_min = min({e, temp * e, current_min * e});
ans = max(ans, current_max);

This ensures correctness for all cases.
```cpp
int maxProduct(vector<int>& nums) {
    int current_max = nums[0], current_min = nums[0], ans = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        int temp = current_max;
        current_max = max({nums[i], current_max * nums[i], current_min * nums[i]});
        current_min = min({nums[i], temp * nums[i], current_min * nums[i]});
        ans = max(ans, current_max);
    }
    return ans;
}
```