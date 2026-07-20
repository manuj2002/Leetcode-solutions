---
problem: "Combination Sum"
difficulty: unknown
verdict: Accepted
runtime: 47 ms
memory: 32.1 MB
date: 2026-07-20
---

# Analysis

### Verdict summary
The approach uses backtracking to explore all combinations by including each candidate multiple times, which is correct for the problem. However, it is inefficient due to multiple vector copies and redundant recursive calls.

### Complexity
Time: O(2^N * T) in the worst case, where N is the number of candidates and T is the target, due to the branching factor. Space: O(T * number of combinations) for storing results and recursion stack.

### vs. optimal
The optimal approach uses standard backtracking without creating vector copies at every step. It typically uses a single vector reference (passed by reference) and pops after recursion to avoid copies. The time complexity is the same worst-case, but it is much faster in practice due to reduced overhead.

### Improvements
1. Avoid creating a new vector `t` for each candidate repetition; instead, use a single vector `v` that is modified and then backtracked (pushed and popped).
2. Eliminate the separate recursive call for skipping the candidate; instead, directly iterate and recurse with the candidate included.
3. Sort the candidates to allow early termination when the remaining target is less than the current candidate.

### Why the percentile is low
The solution creates multiple vector copies (like `vector<int>t=v;`) at each step, which is expensive. Faster solutions use a single vector that is modified in-place and backtracked by popping. Also, sorting the array and breaking early when the candidate exceeds the remaining target improves performance.