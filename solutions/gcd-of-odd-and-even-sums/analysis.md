---
problem: "GCD of Odd and Even Sums"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 7.7 MB
date: 2026-07-15
---

# Analysis

### Verdict summary
The submission uses a straightforward approach by iterating from 1 to n to compute the sums of the first n odd and even numbers, then returns their GCD. While correct, this iterative method is unnecessarily inefficient for the problem constraints (n ≤ 1000) and misses the closed-form formulas that allow constant-time computation.

### Complexity
**Time Complexity:** O(n), due to the loop iterating n times.  
**Space Complexity:** O(1), using only a few integer variables.

### vs. optimal
The optimal approach uses known formulas:  
- Sum of first n odd numbers = n²  
- Sum of first n even numbers = n(n + 1)  
Then computes GCD(n², n(n+1)) = n * GCD(n, n+1). Since GCD(n, n+1) = 1, the result is n. Thus, the answer is simply n.  
This yields O(1) time and space complexity. The submission’s iterative approach is suboptimal for large n (though acceptable here due to constraints) and misses the mathematical insight.

### Improvements
1. **Replace loop with closed-form formulas:** Directly compute `sumOdd = n * n` and `sumEven = n * (n + 1)`.  
2. **Use mathematical identity:** The GCD simplifies to n, so return n directly.  
   Improved code:  
   ```cpp
   int gcdOfOddEvenSums(int n) {
       return n;
   }
   ```

### Why the percentile is low
The fastest solutions leverage the mathematical insight to return n in constant time, avoiding any computation. This submission’s O(n) loop, while acceptable for n ≤ 1000, is slower than the optimal O(1) solution and would be a bottleneck if constraints were higher. The percentile is low because it does unnecessary work.