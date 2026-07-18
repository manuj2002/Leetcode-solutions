---
problem: "Find Greatest Common Divisor of Array"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.2 MB
date: 2026-07-18
---

# Analysis

### Verdict summary
The code correctly computes the GCD of the smallest and largest elements. It uses a single pass to find the min and max, then the built-in `gcd` function. This approach is optimal.

### Complexity
**Time:** O(n + log(min(miNum, maNum))) — one linear pass to find min and max, plus the GCD calculation using Euclidean algorithm.  
**Space:** O(1) — only a few integer variables are used.

### vs. optimal
This solution is optimal. The problem requires finding the GCD of the smallest and largest numbers, which necessitates scanning the array to find them. The Euclidean algorithm for GCD is efficient and standard.

### Improvements
1. **Use initializer list for min/max:** Instead of initializing with `INT_MIN` and `INT_MAX`, use `nums[0]` for both to avoid potential edge cases with extreme values (though not necessary here due to constraints).  
   ```
   int miNum = nums[0], maNum = nums[0];
   for (int i = 1; i < nums.size(); ++i) {
       miNum = min(miNum, nums[i]);
       maNum = max(maNum, nums[i]);
   }
   ```
2. **Explicitly include `<algorithm>`:** Although many environments include it transitively, for portability, add `#include <algorithm>` for `min` and `max`.

### Why the percentile is low
The runtime percentile is misleading—0 ms is the fastest possible and indicates the solution is efficient. The metric is likely based on limited data or noise, as the code is optimal.