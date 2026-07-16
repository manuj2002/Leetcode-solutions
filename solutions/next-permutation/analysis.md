---
problem: "Next Permutation"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 15.7 MB
date: 2026-07-16
---

# Analysis

### Verdict summary

The approach correctly identifies the core steps for finding the next permutation: locating the first decreasing element from the right, then swapping it with the smallest larger element to its right, and finally sorting the suffix. However, the implementation contains an unnecessary initial pass to check if the array is entirely descending, which adds redundant computation. The core logic is correct but not optimally expressed.

### Complexity

**Time Complexity:** O(n log n) in the worst case due to the sorting step. The initial descending check and the index search are O(n), but the sort at the end dominates.  
**Space Complexity:** O(1), as the operations are in-place, meeting the problem's constant extra memory requirement.

### vs. optimal

The known optimal approach is O(n) time and O(1) space. The standard algorithm avoids a full sort by only reversing the suffix after the swap, since the suffix is guaranteed to be in descending order. This reversal is O(n) and avoids the O(n log n) cost of sorting. Your solution differs by using `sort` instead of a simple reverse. While the asymptotic worst-case complexity is worse, it is accepted here due to the small constraint (n ≤ 100). However, for larger inputs, the O(n log n) sort would be noticeably slower than an O(n) reverse.

### Improvements

1.  **Remove redundant descending check:** The initial loop to check if the entire array is descending is unnecessary. The main algorithm (finding index `i`) naturally handles the edge case where the entire array is descending (i.e., when `i` becomes -1). This loop adds an extra O(n) pass.
    *   **Fix:** Delete lines 3-10 entirely.

2.  **Use reverse instead of sort:** After swapping `nums[i]` and `nums[j]`, the subarray from `i+1` to the end is in descending order. Reversing it is sufficient to get the smallest lexicographic order, and it runs in O(n) time versus O(n log n) for sorting.
    *   **Fix:** Replace `sort(nums.begin()+i+1, nums.end());` with `reverse(nums.begin()+i+1, nums.end());`.

3.  **Simplify the swap target selection:** The loop to find index `j` (the smallest element larger than `nums[i]`) is correct but can be optimized. Since the suffix is in descending order, the first element from the right that is larger than `nums[i]` is the smallest valid candidate. This avoids the subtraction comparison.
    *   **Fix:** Replace the loop for finding `j` with:
        ```cpp
        int j = nums.size() - 1;
        while (j > i && nums[j] <= nums[i]) j--;
        ```