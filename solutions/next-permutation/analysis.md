---
problem: "Next Permutation"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-16
---

# Analysis

### Verdict summary
Your approach incorrectly identifies the next permutation by assuming the swap target is always the last element. While it often passes due to the final sort, it fails on cases like `[1,3,2]` (your code would return `[2,1,3]` instead of `[2,3,1]`), so the solution is fundamentally flawed despite being accepted — it’s only correct for a subset of test cases.

### Complexity
**Time:** O(n log n) in most cases due to `sort()`, but up to O(n) in early `idDesc` detection if the array is fully descending (though `idDesc` detection itself is O(n)).  
**Space:** O(1) extra space, meeting the in-place requirement.

### vs. optimal
The known optimal approach: first find the rightmost pair `(i, i+1)` where `nums[i] < nums[i+1]`. Then find the smallest element to the right of `i` that is larger than `nums[i]`, swap them, and finally reverse (not sort) `nums[i+1:]`. This is O(n) time and O(1) space. Your method deviates by incorrectly swapping with the last element and using O(n log n) sorting instead of O(n) reversal for the suffix.

### Improvements
1. **Flawed swap logic** – `swap(nums[i], nums[nums.size()-1])` doesn't guarantee the next permutation. Replace with a search for the rightmost `j > i` such that `nums[j] > nums[i]`, then swap `nums[i]` and `nums[j]`.
2. **Unnecessary `idDesc` pass** – Entire first loop is redundant; the later while loop already finds `i`, and if `i < 0` you can just reverse the whole array.
3. **Sorting instead of reversing** – After swapping, the suffix `nums[i+1:]` is guaranteed to be in descending order initially; reversing it is O(n) and more efficient than O(n log n) sort.
4. **Redundant return statements** – The `return` after `sort` is unnecessary; function ends naturally.

### Why the percentile is low
Even though this got “Accepted”, the hidden test cases likely didn’t expose the logic flaw, but the use of `sort` (O(n log n)) instead of `reverse` (O(n)) and the extra O(n) pass for `idDesc` add overhead. The fastest solutions execute a single pass (or two passes) and a reversal, strictly O(n) with minimal operations.