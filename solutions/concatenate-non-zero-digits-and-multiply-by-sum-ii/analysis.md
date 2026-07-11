---
problem: "Concatenate Non-Zero Digits and Multiply by Sum II"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-11
---

# Analysis

### Verdict summary
The provided code is a skeleton that does not implement any logic and fails to return a value, resulting in a compile error. Even if fixed to return an empty vector, it would be incorrect for the problem. The problem requires efficiently handling up to 10^5 queries on a string of length up to 10^5, which necessitates a precomputation approach.

### Complexity
**Time Complexity:** O(1) per query after preprocessing, but the current code is O(1) total (incorrect).  
**Space Complexity:** O(1) (incorrect).

### vs. optimal
The optimal approach uses prefix sums and prefix products to answer each query in O(1) time after O(n) preprocessing. For a substring [l, r]:
1. **Precompute:**
   - `non_zero_count[i]`: Number of non-zero digits up to index i.
   - `digit_sum[i]`: Sum of non-zero digits up to index i.
   - `digit_value[i]`: The value of the concatenated non-zero digits up to i modulo M, stored as a pair (value, length) to handle concatenation via modular arithmetic.
2. **Query:** Use prefix[r] - prefix[l-1] to get the concatenated number and digit sum for the range. The concatenation is computed as:  
   `x = (prefix_value[r] - prefix_value[l-1] * pow(10, k)) % M`, where k is the number of digits after l.

The current code lacks any implementation and is fundamentally non-viable.

### Improvements
1. **Implement the solution logic:** The function must return a `vector<int>` containing results for all queries. Start by adding `return {};` to fix the compile error, then implement the precomputation and query logic.
2. **Precompute prefix arrays:** Use arrays to store cumulative non-zero digit counts, sums, and concatenated values modulo 10^9+7.
3. **Handle modular arithmetic correctly:** When concatenating digits, use `(prev * 10 + digit) % MOD` and precompute powers of 10 modulo MOD for range queries.
4. **Avoid string manipulation in queries:** Do not extract substrings or process digits naively per query, as that would be O(length) per query and too slow.