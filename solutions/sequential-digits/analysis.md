---
problem: "Sequential Digits"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-13
---

# Analysis

### Verdict summary
This solution generates all sequential-digit numbers by brute-force constructing every possible starting digit and length, then filters them against the bounds. While it passes due to extremely small total numbers (only 36 possible sequential numbers total in the range 10–10^9), it includes unnecessary steps: it generates many numbers outside the target length range and performs a separate filtering pass.

### Complexity
- Time: O(1) — because there are only 9·10/2 = 45 possible sequential-digit numbers with ≤ 10 digits, and the loops iterate at most 9×9 = 81 times regardless of input.
- Space: O(1) ignoring output, since intermediate vector `p` stores at most 45 ints.

### vs. optimal
This is essentially optimal in complexity (there are only O(1) valid numbers), but the implementation is not the most direct known optimal approach. The optimal solution typically generates numbers in order by iterating over starting digits (1–9) and length (2–10 digits), constructing each sequential number, adding it if within bounds, and stopping early when the number exceeds `high` or length exceeds 10. This avoids storing and later filtering intermediate results.

### Improvements
1. **Remove the separate filter pass:** Instead of storing all candidates in `p` then filtering by range, check `low ≤ t ≤ high` while generating and push to `ans` directly. This saves extra storage and a full pass.
2. **Avoid generating out-of-range lengths:** The inner `while(x<=9)` can stop early when starting digit `x` causes the generated number to exceed 10-digit constraints or when initial digit too high for length `i` (this is minor but more logical).
3. **Remove debugging output:** `cout<<t<<endl;` should be deleted in final code.
4. **Better naming:** `p` is unclear; use `candidates` or just eliminate it entirely. `digitCounter` is fine but `countDigits` more conventional.
5. **Use direct length limits:** Instead of `for(int j=x; j<=9 && cnt<i; j++)`, compute max possible starting digit for given length as `10-i` to skip pointless loops.