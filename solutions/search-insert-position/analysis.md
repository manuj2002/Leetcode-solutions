---
problem: "Search Insert Position"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-09
---

# Analysis

### Verdict summary
The submitted code uses a linear scan approach, which is incorrect for this problem. The problem explicitly requires O(log n) runtime complexity, but this solution runs in O(n) time. Although it passed the test cases due to small constraints, it would fail on larger inputs.

### Complexity
- **Time complexity:** O(n) — the loop iterates through the entire array in the worst case (e.g., when the target is larger than all elements).
- **Space complexity:** O(1) — only a constant amount of extra space is used.

### vs. optimal
The optimal approach for this problem is **binary search**, which achieves O(log n) time complexity by repeatedly halving the search interval. The submitted solution uses a linear scan, which is strictly worse asymptotically. Binary search is mandatory here due to the problem's constraints and requirements.

### Improvements
1. **Replace linear scan with binary search:** The entire loop should be replaced with a binary search implementation. For example:
   ```cpp
   int low = 0, high = nums.size() - 1;
   while (low <= high) {
       int mid = low + (high - low) / 2;
       if (nums[mid] == target) return mid;
       if (nums[mid] < target) low = mid + 1;
       else high = mid - 1;
   }
   return low;
   ```
2. **Remove redundant variable:** The `ans` variable is unnecessary and confusing; the insertion point is naturally found when the binary search terminates.

### Why the percentile is low
Faster solutions use binary search to achieve O(log n) time, while this solution uses O(n) linear scanning. Even though it runs in 0 ms on small test cases, it would be significantly slower on larger inputs, explaining the low runtime percentile. The memory usage is optimal, but the inefficient algorithm drags down the overall performance ranking.