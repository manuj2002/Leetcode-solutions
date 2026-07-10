---
problem: "Kth Ancestor of a Tree Node"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-10
---

# Analysis

### Verdict summary
The submission implements binary lifting correctly in concept, but contains an indexing error causing buffer overflow. The approach is appropriate for handling multiple queries efficiently, but the implementation is flawed.

### Complexity
- **Time complexity**: Constructor O(n log n), getKthAncestor O(log k)
- **Space complexity**: O(n log n) for the DP table

### vs. optimal
This is the optimal approach for this problem. Binary lifting preprocesses ancestor information in logarithmic steps, allowing each query to be answered in O(log k) time. However, the implementation fails to handle edge cases where intermediate ancestors don't exist.

### Improvements
1. **Fix index out-of-bounds access**: In `getKthAncestor`, when `dp[i][ans] == -1`, the function should return -1 immediately instead of continuing. Also, the loop should check bounds before accessing `dp[i][ans]`.
2. **Correct DP table dimensions**: The constructor initializes with `LOG+1` rows, but the loop runs `for(int i=1;i<LOG;i++)`, missing the last row. It should be `for(int i=1;i<=LOG;i++)`.
3. **Bounds checking in constructor**: When computing `dp[i][j] = dp[i-1][dp[i-1][j]]`, verify that `dp[i-1][j]` is not -1 first.
4. **Use unsigned integers carefully**: Avoid potential overflow in the logarithm calculation by using integer arithmetic more carefully.

Here's the corrected main logic:
```cpp
int getKthAncestor(int node, int k) {
    int ans = node;
    for(int i = LOG; i >= 0; i--) {
        if(k >= (1 << i)) {
            if(dp[i][ans] == -1) return -1;
            ans = dp[i][ans];
            k -= (1 << i);
        }
    }
    return ans;
}
```

And in the constructor:
```cpp
for(int i = 1; i <= LOG; i++) {
    for(int j = 0; j < n; j++) {
        if(dp[i-1][j] == -1) {
            dp[i][j] = -1;
        } else {
            dp[i][j] = dp[i-1][dp[i-1][j]];
        }
    }
}
```