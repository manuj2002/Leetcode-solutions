---
problem: "House Robber"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-26
---

# Analysis

### Verdict summary
The submission attempts a dynamic programming approach but contains a critical out-of-bounds access error for small inputs. The core idea of storing subproblem solutions is correct, but the implementation fails to handle the base case where the input has only one house, causing a buffer overflow.

### Complexity
- **Time complexity**: O(n²) due to the nested loop where for each house `i`, it scans all houses from `i-2` down to `0`.
- **Space complexity**: O(n) for the `dp` array.

### vs. optimal
The optimal solution uses O(n) time and O(1) space with two variables tracking the maximum profit up to the previous two houses. This submission differs by:
1. Using unnecessary O(n) space when O(1) suffices.
2. Implementing an O(n²) recurrence instead of the standard O(n) DP transition `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.

### Improvements
1. **Fix buffer overflow**: The code assumes `nums.size() >= 2` by accessing `dp[1]`. Add a check:  
   ```cpp
   if (n == 1) return nums[0];
   ```
2. **Optimize DP recurrence**: Replace the O(n²) inner loop with O(1) transitions:  
   ```cpp
   dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
   ```
3. **Reduce space to O(1)**: Use two variables instead of a full `dp` array:
   ```cpp
   int prev2 = 0, prev1 = nums[0];
   for (int i = 1; i < n; i++) {
       int curr = max(prev1, prev2 + nums[i]);
       prev2 = prev1;
       prev1 = curr;
   }
   return prev1;
   ```