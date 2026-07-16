---
problem: "Next Permutation"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-16
---

# Analysis

### Verdict summary
The attempted approach tries to detect if the array is descending to decide when to sort, then finds the first decreasing element from the right to swap and sort. However, it fails due to variable scope errors (undeclared `i`), incorrect descending detection logic, and flawed swap choice. This isn’t the correct implementation of the standard next permutation algorithm.

### Complexity
Time: O(n log n) (from sorting). Space: O(1) aside from sort's stack space. The detection loop is O(n).

### vs. optimal
The known optimal approach is:
1. Find the first index `i` from the right where `nums[i] < nums[i+1]`.
2. If none exists, reverse the entire array (lowest permutation).
3. Otherwise, find the smallest element larger than `nums[i]` to the right, swap them.
4. Reverse `nums[i+1..end]` (or sort, but reverse is O(n) here since it’s already descending).

Optimal time O(n), space O(1). This submission incorrectly uses sorting throughout, doesn’t reverse efficiently, and the descending check (which is wrong anyway) is unnecessary—the problem naturally handles the "wrap-around" case in step 2.

### Improvements
1. **Variable scope**: `i` used in the second while loop is undeclared; it should be `int i = n;` given earlier `n = nums.size()-2`? But `n` might be set wrong. Declare `i` explicitly.
2. **Faulty descending detection**: The `if(nums[i-1]>=nums[i])` check in the first loop causes an out-of-bounds access when `i=0` and doesn’t correctly test descending order. Remove this entire detection block—it’s not needed.
3. **Swap target**: `swap(nums[i], nums[nums.size()-1])` is arbitrary; you must find the smallest larger element to the right of `nums[i]` and swap with that, not the last element.
4. **Unnecessary sorts**: Instead of `sort(nums.begin()+i, nums.end())`, reverse `nums.begin()+i+1, nums.end()` after swapping for O(n) worst-case. Sorting is O(n log n) overkill.