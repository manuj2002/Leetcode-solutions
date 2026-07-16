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
The submission attempts the classic next permutation algorithm but has a critical logic error and a compile error due to scoping. The overall approach (find dip, swap, reverse suffix) is correct in intent but poorly executed.

### Complexity
- **Time:** O(n log n) due to the initial descending check sort and potential extra sort at the end; plus O(n) scans.
- **Space:** O(1) extra space aside from sort internals (which may use O(log n) or O(n) depending on implementation).

### vs. optimal
The optimal approach is O(n) time and O(1) extra space:
1. Find the largest index `i` where `nums[i] < nums[i + 1]`.
2. If none, reverse whole array.
3. Find largest index `j > i` where `nums[j] > nums[i]`.
4. Swap `nums[i]` and `nums[j]`.
5. Reverse `nums[i+1:]`.

This submission’s logic is flawed: initial descending check is unnecessary O(n log n), finding the swap partner uses difference comparison (fails with negative differences, though constraints avoid this here), and mistakenly uses `k` outside its scope.

### Improvements
1. **Remove preliminary descending check and unnecessary sorts**: Instead, handle the "no next permutation" case by reversing in step 2 of optimal algorithm.
2. **Fix scoping error**: Line 33 `swap(nums[i], nums[k])` – `k` is out of scope. Use `j`, which was intended to hold the swap index.
3. **Replace difference comparison with direct value comparison**: `nums[j] > nums[i]` is sufficient for finding the smallest larger element if scanning backwards; but here scanning forwards wrongly picks the last larger element. Change to scanning from end to find the smallest element larger than `nums[i]`.
4. **Use reverse instead of sort for suffix**: After swap, the suffix `nums[i+1:]` is guaranteed to be non-increasing; reversing it yields sorted ascending order in O(n) vs. O(n log n).