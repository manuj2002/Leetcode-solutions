---
problem: "Search Insert Position"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-09
---

# Analysis

### Verdict summary

The submitted code implements a linear scan through the array to find the target or its insertion position. While this solution is functionally correct and accepted, it violates the problem's explicit requirement of O(log n) runtime complexity. The linear scan results in O(n) time, which is inefficient for the constraints (n up to 10^4).

### Complexity

- **Time Complexity:** O(n) — The loop iterates through the entire array in the worst case.
- **Space Complexity:** O(1) — Only a fixed number of integer variables are used.

### vs. optimal

The known optimal approach is binary search, which achieves O(log n) runtime complexity by repeatedly halving the search space. The submitted linear scan is fundamentally different and suboptimal, as it does not leverage the sorted property of the array to reduce comparisons.

### Improvements

1. **Replace linear scan with binary search.** This is the most critical improvement. The algorithm should maintain two pointers (`low` and `high`) and calculate a `mid` index to narrow the search range.
   
   Example binary search implementation:
   ```cpp
   int low = 0, high = nums.size() - 1;
   while (low <= high) {
       int mid = low + (high - low) / 2;
       if (nums[mid] == target) return mid;
       else if (nums[mid] < target) low = mid + 1;
       else high = mid - 1;
   }
   return low;
   ```

2. **Use meaningful variable names.** `ans` is vague; `insertPos` would be clearer. However, the primary issue is algorithmic, not naming.

### Why the percentile is low

The runtime percentile is low because this solution runs in O(n) time, while faster submissions use binary search to achieve O(log n) time. For an array of size 10^4, binary search requires ~14 comparisons, whereas the linear scan may require up to 10,000. The performance gap is significant for larger inputs, even within the constraint limits.