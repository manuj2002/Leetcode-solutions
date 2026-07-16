---
problem: "Next Permutation"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.4 MB
date: 2026-07-16
---

# Analysis

### Verdict Summary
Your solution attempts to simulate the lexicographic algorithm but has critical logic errors and implements unnecessary preprocessing. It correctly identifies the descending-suffix case but is flawed in its next-permutation logic; the final accepted verdict is due to small constraints, not robust logic.

### Complexity
- Time: O(n log n) worst case due to sorting in the descending check; typical case O(n^2) from the find-and-swap loop with inner comparisons.
- Space: O(1) extra space, meeting the constant-memory requirement.

### vs. Optimal
Standard optimal approach:  
1. Starting from the right, find the first index `i` where `nums[i] < nums[i+1]`.  
2. Find the smallest element larger than `nums[i]` in the suffix.  
3. Swap them and reverse the suffix (instead of sorting or partial reverse).  
Time: O(n), space: O(1).  
Your code incorrectly uses difference comparison (`nums[j] - nums[i] > nums[k] - nums[i]`) — unstable with negative differences. The final reversal loop also incorrectly swaps `nums[i]` multiple times.

### Improvements
1. **Incorrect swap choice (critical)**:  
Change `if (nums[j] - nums[i] > nums[k] - nums[i])` to `if (nums[k] <= nums[j] && nums[k] > nums[i])` or simply iterate from the right to find the first element `> nums[i]`.  
2. **Broken final reversal**:  
The second `while(j <= k)` incorrectly uses `swap(nums[i], nums[j])`; it should reverse `nums[i+1:]` with `swap(nums[j], nums[k])`.  
3. **Unneeded descending check**:  
Remove `idDesc` scan and sort — optimal approach handles descending case with step 1.  
4. **Unidiomatic C++**:  
Use `std::reverse()` instead of manual reverse.

### Why the percentile is low
Faster solutions (O(n)) avoid redundant passes and use `std::reverse`. Yours incurs redundant loops, incorrect calculations (difference comparisons), an unnecessary O(n log n) sort in one case, and uses O(n²) worst-case loops. Most importantly, it’s logically unsound and risks failure for certain inputs, though specific examples may pass due to small constraints.