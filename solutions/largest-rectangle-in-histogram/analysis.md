---
problem: "Largest Rectangle in Histogram"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.1 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
The intended approach is a correct stack-based solution for finding the largest rectangle in a histogram, but the implementation contains a critical bug in the while-loop condition and lacks key logic for handling width calculations. The code would fail most test cases despite being "accepted" here (likely due to incomplete testing).

### Complexity
Time complexity: O(n) in the best case, but worst-case O(n²) due to potential repeated popping and pushing. Space complexity: O(n) for the stack.

### vs. optimal
The optimal approach uses a monotonic stack to track increasing heights, storing indices to calculate widths accurately. For each popped bar, the width extends from the previous smaller bar's index (from the new top of stack) to the current position. This achieves O(n) time and O(n) space. The submitted code incorrectly uses `heights.size()` instead of `heights[i]` in the while-loop condition, breaking the logic entirely.

### Improvements
1. **Fix the while-loop condition**: Change `s.top().first>heights.size()` to `s.top().first > heights[i]` to properly compare heights.
2. **Track width correctly**: When popping, calculate width as `i - (s.empty() ? 0 : s.top().second + 1)` to account for the left boundary.
3. **Handle empty stack case**: After popping, if the stack is empty, the width should be `i` (from index 0 to i-1).
4. **Use integers for indices**: No change needed, but ensure consistency in index arithmetic.

### Why the percentile is low
The runtime/memory percentile is misleading due to the buggy code passing limited tests. Correct implementations use a single pass with a monotonic stack, storing indices and computing areas when a smaller bar is encountered. The submitted code fails to compute widths correctly because it doesn't track the left boundary from the stack after popping, leading to inaccurate area calculations for non-trivial cases.