---
problem: "Majority Element II"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.4 MB
date: 2026-07-17
---

# Analysis

### Verdict summary
The solution uses the Boyer-Moore Majority Vote algorithm for two candidates, which is the optimal approach for linear time and constant space. It correctly identifies potential candidates and verifies their frequencies, but the implementation has a logical flaw in the candidate selection phase.

### Complexity
Time: O(n) — Two passes through the array.
Space: O(1) — Uses only constant extra space for counters and variables.

### vs. optimal
The Boyer-Moore Majority Vote algorithm is optimal for this problem, achieving O(n) time and O(1) space. However, the implementation contains a critical issue: the assignment of `item1` and `item2` is mishandled. The initial condition `if (e != item1 && count2 == 0)` prioritizes filling `item2` when `count2` is zero, which is incorrect. Standard implementations prioritize filling the first candidate slot when its count is zero, not the second. This may lead to incorrect candidate selection in some cases.

### Improvements
1. **Fix candidate selection logic**: The conditions in the first loop should be symmetric. Standard approach:
   ```cpp
   if (count1 == 0 && e != item2) {
       item1 = e;
       count1 = 1;
   } else if (count2 == 0 && e != item1) {
       item2 = e;
       count2 = 1;
   } else if (e == item1) {
       count1++;
   } else if (e == item2) {
       count2++;
   } else {
       count1--;
       count2--;
   }
   ```
2. **Avoid redundant checks**: The verification loop uses two separate `if` statements, which is correct. However, the condition `if (cnt2 >= mini && item1 != item2)` is necessary to avoid duplicates only if `item1` and `item2` are the same. But since the algorithm should ensure distinct candidates, this is a safeguard. Alternatively, you can initialize `item1` and `item2` to distinct values (e.g., different integers) to avoid this, but it's not strictly necessary.

### Why the percentile is low
Despite being accepted, the runtime percentile is low because the implementation has a suboptimal candidate selection order. The flawed logic may cause unnecessary decrements or missed candidates in edge cases, though it passed the test suite. Corrected implementations would be more efficient and robust. Additionally, the solution uses two full passes (which is standard), but some solutions might combine verification with candidate selection in one pass for minor gains, though this is not significant.