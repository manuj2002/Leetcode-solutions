---
problem: "Assign Cookies"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-27
---

# Analysis

### Verdict summary
The submission attempts a greedy two-pointer approach but contains a critical logical error that leads to accessing invalid indices. The core idea (sorting and matching greed factors with cookie sizes from largest to smallest) is correct, but the loop condition and pointer updates are flawed.

### Complexity
- **Time complexity:** O(g log g + s log s) due to sorting, where g and s are the sizes of the input vectors. The while loop runs in O(g + s) time, so sorting dominates.
- **Space complexity:** O(1) extra space (ignoring the space used by sorting).

### vs. optimal
Your approach is *conceptually* optimal for this problem. The known optimal solution is to sort both arrays and use two pointers (either from smallest to largest or largest to smallest) to greedily assign the smallest sufficient cookie to each child or the largest cookie to the most demanding child first. Your implementation differs in a critical flaw: the loop condition `while(i>=0 || j>=0)` and the unconditional `i--` allow the index `i` (for children) to be decremented even when `j` (for cookies) is already negative, leading to an attempt to access `g[-1]` when `i` becomes negative and `j` is negative.

### Improvements
1.  **Fix the loop condition and pointer logic:** The main issue is that `i` is decremented on every iteration, regardless of whether a cookie was assigned. Furthermore, the loop continues even when one pointer is exhausted. The correct logic for the "largest-to-smallest" approach is to stop when *either* pointer is exhausted. Also, only decrement `j` (the cookie index) when an assignment is made. A corrected version would be:
    ```cpp
    int i = g.size() - 1, j = s.size() - 1;
    while (i >= 0 && j >= 0) { // Stop when we run out of children OR cookies
        if (s[j] >= g[i]) {
            ans++;
            j--; // Only use the cookie if it's assigned
        }
        i--; // Always move to the next child
    }
    ```

2.  **Avoid negative indices:** The original code's use of `||` in the loop condition allows the loop to continue when `i` is negative, accessing `g[i]` which is undefined behavior. The fix in point #1 prevents this by requiring both indices to be non-negative.

3.  **Clarity of intent:** The small-to-large greedy approach is often more intuitive and has identical complexity. It involves sorting both arrays and using two pointers starting from the beginning, trying to assign the smallest cookie that satisfies each child. This logic is often easier to write correctly on the first try.