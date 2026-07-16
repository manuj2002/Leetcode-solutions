---
problem: "Next Permutation"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.4 MB
date: 2026-07-16
---

# Analysis

### Verdict summary
The submission attempts to implement the standard algorithm, but contains a critical misunderstanding. It erroneously assumes the entire array must be checked for being strictly descending, which is inefficient and incorrect for partial permutations. The core reversal logic is close but flawed.

### Complexity
**Time Complexity:** O(n log n) worst-case due to the `sort` operations. The initial linear check is O(n).  
**Space Complexity:** O(1) extra space (ignoring the sort's stack usage), meeting the in-place requirement.

### vs. optimal
The optimal approach is O(n) time and O(1) space. It involves:
1. Finding the largest index `i` where `nums[i] < nums[i+1]` (starting from the right).
2. Finding the largest index `j` where `nums[j] > nums[i]` (starting from the right).
3. Swapping `nums[i]` and `nums[j]`.
4. Reversing the subarray from `i+1` to the end.

The submitted code is suboptimal. It uses an unnecessary O(n) pass to check if the array is entirely descending (which is irrelevant for partial permutations) and replaces the O(n) reversal with an O(n log n) sort.

### Improvements
1. **Remove the initial descending check.** The `idDesc` logic is flawed and unnecessary. The main algorithm should handle all cases starting from Step 1.
2. **Find the correct swap partner.** After finding index `i`, you must find the *smallest number larger than nums[i]* to its right (not just swap with the last element).
3. **Use reversal instead of sort.** After swapping, the suffix is guaranteed to be in descending order. A simple reverse (O(n)) is sufficient and faster than sorting (O(n log n)).
4. **Fix the edge case logic.** The `if(i<0) return;` is correct for the "last permutation" case but is contingent on fixing the preceding steps.

Revised core logic:
```cpp
int i = nums.size() - 2;
while (i >= 0 && nums[i] >= nums[i + 1]) i--;
if (i >= 0) {
    int j = nums.size() - 1;
    while (nums[j] <= nums[i]) j--;
    swap(nums[i], nums[j]);
}
reverse(nums.begin() + i + 1, nums.end());
```

### Why the percentile is low
Faster solutions (0ms) implement the standard O(n) algorithm correctly. This submission's unnecessary initial pass and use of `sort` instead of `reverse` add avoidable overhead, causing it to be slower in practice despite the O(n log n) worst-case being acceptable for small constraints. The algorithm's fundamental flaw in the swap step also indicates it would fail on many test cases not shown here.