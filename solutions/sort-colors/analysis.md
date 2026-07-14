---
problem: "Sort Colors"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 11.8 MB
date: 2026-07-14
---

# Analysis

### Verdict summary
The submitted solution uses a two-pass counting sort. It first counts the occurrences of 0s, 1s, and 2s, then overwrites the array with the counts in order. While this approach works and is O(n), it is not the optimal one-pass solution requested in the follow-up.

### Complexity
**Time complexity:** O(n) — Two passes through the array: one for counting and one for overwriting.  
**Space complexity:** O(1) — Uses only three integer counters (z, o, t), so constant extra space.

### vs. optimal
The optimal approach for this problem is the Dutch National Flag algorithm, which achieves O(n) time complexity in a single pass using three pointers (low, mid, high) to partition the array into 0s, 1s, and 2s in-place. The submitted solution differs by requiring two passes and is less efficient for large datasets due to unnecessary overwrites, even though it meets the base problem constraints.

### Improvements
1. **Adopt the Dutch National Flag algorithm** for a true one-pass solution. Replace the counting approach with:
   ```cpp
   int low = 0, mid = 0, high = nums.size() - 1;
   while (mid <= high) {
       if (nums[mid] == 0) swap(nums[low++], nums[mid++]);
       else if (nums[mid] == 1) mid++;
       else swap(nums[mid], nums[high--]);
   }
   ```
2. **Avoid redundant overwrites**. The current method writes all elements twice (once to count, once to assign), which is inefficient for large arrays.
3. **Use pre-increment where possible**. In the reassignment loop, `z--`, `o--`, and `t--` could be merged into the assignment (e.g., `nums[i] = (z-- > 0) ? 0 : ...`), though this is minor.