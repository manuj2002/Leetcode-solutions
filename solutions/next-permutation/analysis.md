---
problem: "Next Permutation"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-16
---

# Analysis

### Verdict summary
The approach attempts to find the next lexicographical permutation but has critical flaws. The initial loop checks for a descending array incorrectly and a runtime error occurs when accessing `nums[i-1]` on the first iteration (i=0). The core logic is incomplete even if fixed.

### Complexity
- **Time complexity:** O(N log N) due to sorting operations.
- **Space complexity:** O(1) additional space, meeting the constant-memory requirement.

### vs. Optimal
The optimal approach for this problem (standard algorithm) is O(N) time and O(1) space:
1. Find the largest index `i` such that `nums[i] < nums[i+1]` (from the end).
2. Find the smallest element larger than `nums[i]` to its right.
3. Swap them.
4. Reverse the suffix after `i` to get the smallest lex order.

Your code fails to correctly identify the pivot point and doesn't handle the swap-and-reverse logic properly. The incorrect initial descending check and sorting instead of reversing make it suboptimal.

### Improvements
1. **Fix index bounds:** Remove the first loop entirely. The `i-1` access at `i=0` causes undefined behavior.
2. **Correct pivot search:** Start from the end to find the first decreasing element (`nums[i] < nums[i+1]`).
3. **Find correct swap target:** Search from the end for the smallest element larger than `nums[i]`, then swap.
4. **Reverse suffix:** After swapping, reverse the subarray from `i+1` to end (O(N)) instead of sorting (O(N log N)).

Example corrected implementation:
```cpp
void nextPermutation(vector<int>& nums) {
    int i = nums.size() - 2;
    while (i >= 0 && nums[i] >= nums[i + 1]) i--;
    if (i >= 0) {
        int j = nums.size() - 1;
        while (j > i && nums[j] <= nums[i]) j--;
        swap(nums[i], nums[j]);
    }
    reverse(nums.begin() + i + 1, nums.end());
}
```