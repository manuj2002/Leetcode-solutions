---
problem: "Smallest Palindromic Rearrangement I"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 7.8 MB
date: 2026-07-28
---

# Analysis

### Verdict summary
The submission attempts to solve the problem by counting character frequencies and constructing the result from the halves. However, the implementation has critical logic errors in handling odd-frequency characters and building the second half, which coincidentally pass the examples but would fail on many cases. The core idea is correct but flawed in execution.

### Complexity
- **Time complexity:** O(n + 26) → O(n), where n is the length of the string. This is optimal asymptotically.
- **Space complexity:** O(26) → O(1) for frequency array, plus O(n) for the output string.

### vs. optimal
The optimal approach for this problem is to:
1. Count character frequencies.
2. Build the first half lexicographically by placing characters in ascending order, using half of each frequency.
3. For the middle character (if any), choose the smallest character with an odd frequency.
4. Mirror the first half to form the palindrome.

This submission mistakenly:
- Uses variable `c` to store the *last* odd-frequency character encountered, not the *smallest*. This would fail for inputs like "aabbb" (should yield "abbba", but here might yield "babab").
- Reuses the frequency array incorrectly when building the second half: it appends remaining counts instead of mirroring the first half.

### Improvements
1. **Fix odd-character selection:** Track the smallest character with odd frequency, not the last one.
2. **Mirror the first half:** After building the first half, reverse it for the second half—don’t reuse frequency counts.
3. **Use clearer loops:** Replace manual `while` loops with `string(freq[i], 'a' + i)` for conciseness.
4. **Avoid redundant work:** Skip the second frequency loop entirely by reversing the first half.

### Why the percentile is low
The runtime percentile is low because the current logic is flawed (despite passing examples). Efficient solutions correctly mirror the first half and handle odd frequencies lexicographically. This submission’s erroneous approach would fail on hidden test cases, but even if corrected, it may still be slower due to:
- Inefficient character-by-character appends instead of bulk string operations.
- Extra passes over the frequency array. Optimal solutions build the result in one pass over the alphabet and mirror it instantly.