---
problem: "Maximum Product of Two Elements in an Array"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 13.4 MB
date: 2026-07-27
---

# Analysis

### Verdict summary
Your solution sorts the array to pick the two largest values, then calculates the required product. This is a correct but not optimal approach in terms of time complexity for the given constraints.

### Complexity
- **Time complexity:** O(_n_ log _n_), due to sorting the entire array.
- **Space complexity:** O(log _n_) for the sort's stack space (or O(1) if ignoring recursion overhead), as the sort is in-place.

### vs. optimal
The optimal solution runs in O(_n_) time and O(1) space, using a single pass to find the two largest numbers without sorting. Sorting the entire array is overkill; you only need the top two values.

### Improvements
1. **Replace sorting with a linear scan.** Sorting is O(_n_ log _n_) when O(_n_) is sufficient. Maintain `max1` and `max2` in one pass.
2. **Avoid indexing by `.size()-1`.** You can use `.back()` and `secondLast = *(nums.rbegin()+1);` with iterators, but the clarity difference is minor. The main optimization is algorithmic.

Example O(_n_) fix:
```cpp
int max1 = 0, max2 = 0;
for (int x : nums) {
    if (x > max1) {
        max2 = max1;
        max1 = x;
    } else if (x > max2) {
        max2 = x;
    }
}
return (max1 - 1) * (max2 - 1);
```