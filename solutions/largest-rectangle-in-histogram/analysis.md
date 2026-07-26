---
problem: "Largest Rectangle in Histogram"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.3 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
The solution uses a stack to process bars in increasing height order but incorrectly tracks the starting index for each height. The core idea is correct (monotonic stack), but the implementation's indexing error leads to incorrect area calculations after pops, causing the final cleanup to be wrong in general cases.

### Complexity
Time: O(n) because each element is pushed and popped at most once.  
Space: O(n) worst-case for the stack.

### vs. optimal
The optimal approach is indeed a monotonic stack (O(n) time, O(n) space). However, the correct implementation must store indices and compute widths by using the previously stored index when a bar is popped, effectively tracking how far left the current height can extend. This submission incorrectly assigns `i` as the starting index when pushing, and in the final cleanup it incorrectly uses `n-0` instead of `n - stored_index`.

### Improvements
1. **Fix width calculation on pop**: When popping, the width should be `(i - stored_index)`, where `stored_index` is the index saved earlier when that height started expanding leftwards. The current code uses `i - s.top().second` incorrectly because `s.top().second` may not be the true left bound after intermediate pops.
2. **Final cleanup calculation**: The final loop incorrectly uses `(n-0)`—it should compute `(n - stored_index) * height` for each remaining stack entry.
3. **Use integers only**: Using a `pair<int,int>` is fine, but a single stack of indices referencing `heights` is more common and avoids storing duplicate heights.
4. **Add sentinel**: Append a zero-height bar to force a final cleanup and simplify the code.

### Why the percentile is low
Though runtime shows 0 ms, typical correct O(n) monotonic stack solutions track the proper left boundary by retaining the index of the last popped bar when pushing a new shorter bar. This submission fails on cases like `[2,1,2]` (should return 3, but would compute incorrectly). Benchmark runs may not catch this due to limited test cases, but the flaw is present.