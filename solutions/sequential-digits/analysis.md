---
problem: "Sequential Digits"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8 MB
date: 2026-07-13
---

# Analysis

### Verdict summary
The submission attempts to generate sequential numbers by constructing numbers where all digits are equal (e.g., 11, 22, 333) instead of actually generating sequential digits (like 123, 234). The approach is fundamentally flawed and does not solve the problem as intended, but the code was accepted due to weak test cases.

### Complexity
- **Time complexity**: The outer loop runs over digit lengths (l to r), and inner loops run a constant number of times (each loop bounded by 9). However, the inner construction is incorrect. The total operations are O(1) due to fixed constraints (only 9 possible starting digits and max 9 lengths).
- **Space complexity**: O(1) for storage (only 36 numbers maximum in `p`), but the output list size is bounded by the number of sequential digits in the range (which is also O(1)).

### vs. optimal
The optimal approach generates all sequential numbers by iterating over possible starting digits (1-9) and possible lengths (2-9), constructing each number by incrementing the last digit. For example, start with 1, then form 12, 123, etc., until digit >9. This generates all valid numbers without duplicates, and they are naturally sorted. The complexity is O(1) since there are only 36 such numbers total (9+8+7+...+1). The submission does not generate sequential digits at all—it generates repdigits (like 11, 22)—so it is incorrect and only passed due to coincidental test cases.

### Improvements
1. **Correct the generation logic**: Replace the inner loops to actually generate sequential digits. For each starting digit `s` and length `len`, build the number by appending `s + i` for i in [0, len-1].
2. **Avoid redundant collection**: Instead of first collecting all repdigits and then filtering, generate only valid sequential digits directly within the range.
3. **Use efficient construction**: Precompute the sequential numbers without storing intermediate incorrect values.

Fixed code snippet:
```cpp
vector<int> sequentialDigits(int low, int high) {
    vector<int> ans;
    for (int start = 1; start <= 9; ++start) {
        int num = start;
        for (int next = start + 1; next <= 9; ++next) {
            num = num * 10 + next;
            if (num >= low && num <= high) {
                ans.push_back(num);
            }
        }
    }
    sort(ans.begin(), ans.end());
    return ans;
}
```
This generates all sequential digits of length >=2 and sorts them (though they are naturally generated in order, so sorting might be unnecessary but safe). Alternatively, generate by length first to avoid sorting.

### Why the percentile is low
The code is incorrect and only passed due to weak test cases (e.g., the provided examples only contain numbers like 123 and 234, which are repdigits only when the starting digit is repeated? Actually, 123 is not a repdigit). The runtime and memory are good only because the problem size is small, but the solution is wrong. Faster solutions correctly generate sequential digits without any extraneous computation or storage.