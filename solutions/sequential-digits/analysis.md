---
problem: "Sequential Digits"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.1 MB
date: 2026-07-13
---

# Analysis

### Verdict summary
The approach attempts to generate all sequential numbers by iterating over possible digit lengths and starting digits, then filtering for the valid range. While the core idea is in the right direction, the implementation has logical flaws that cause it to generate incorrect numbers and perform unnecessary work.

### Complexity
- **Time complexity**: O(1) in practice, since the number of sequential digits is fixed (only 36 possible numbers between 10 and 10^9).
- **Space complexity**: O(1), as the output list has at most 36 elements.

### vs. optimal
The optimal approach is to precompute *all* 36 possible sequential-digit numbers (from 12 to 123456789) and then filter those within [low, high]. This takes O(1) time and space. 

Your approach is suboptimal because:
- The inner loop logic is flawed: it generates numbers with *exactly* `i` digits, but the loop condition `cnt <= i` allows generating numbers with fewer digits if `j` exceeds 9 prematurely. This produces incorrect numbers.
- It uses an intermediate list `p` to store *all* generated numbers of lengths between `l` and `r`, many of which may be invalid, before filtering.

### Improvements
1. **Fix the generation logic**: The inner loop should explicitly generate numbers of length `i` only. For example, for length 3, start with digit `d` and take exactly 3 digits: `d, d+1, d+2`. Stop if `d + i - 1 > 9`.
2. **Avoid intermediate storage**: Generate numbers and directly check if they are within [low, high], instead of storing all candidates first.
3. **Simplify digit counting**: Use `to_string(low).length()` and `to_string(high).length()` for clarity.
4. **Use a cleaner loop structure**: Iterate over starting digits (1–9) and lengths (2–9), breaking early when the sequential digit exceeds 9.

Example corrected snippet:
```cpp
for (int len = l; len <= r; len++) {
    for (int start = 1; start <= 10 - len; start++) {
        int num = 0;
        for (int j = 0; j < len; j++) 
            num = num * 10 + (start + j);
        if (num >= low && num <= high) 
            ans.push_back(num);
    }
}
```

### Why the percentile is low
Faster solutions precompute all 36 valid sequential numbers once (e.g., in a static vector) and simply iterate through them to filter by range. This avoids redundant digit-by-digit construction for each query and has minimal branching. Your solution’s flawed logic and unnecessary intermediate storage cause it to be slower in practice, despite the same asymptotic complexity.