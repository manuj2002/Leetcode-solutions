---
problem: "Find Greatest Common Divisor of Array"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 16.2 MB
date: 2026-07-18
---

# Analysis

### Verdict summary
This submission correctly identifies the smallest and largest elements in one pass and computes their GCD using the `gcd` function. The approach is optimal, as the problem reduces to finding the GCD of the min and max of the array.

### Complexity
- **Time Complexity:** O(n + log(min(maNum, miNum))). The single pass through the array is O(n). The `gcd` function using the Euclidean algorithm has a time complexity of O(log(min(a, b))).
- **Space Complexity:** O(1). Only a few integer variables are used.

### vs. optimal
This solution is optimal. The problem requires finding the minimum and maximum elements and computing their GCD. The submission does this with O(n) time and O(1) space, which is the best possible complexity.

### Improvements
No major issues exist. However, two minor points could slightly improve code quality:
1.  **Variable Naming:** `maNum` and `miNum` should be renamed to `maxNum` and `minNum` for immediate clarity.
2.  **Initialization:** `INT_MIN`/`INT_MAX` is correct, but initializing `minNum` to `nums[0]` and `maxNum` to `nums[0]` is a common, equally efficient alternative.