---
problem: "Smallest Palindromic Rearrangement I"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-28
---

# Analysis

### Verdict summary

The submitted code attempts to rearrange characters to form the lexicographically smallest palindrome by counting frequencies and building the string in three parts: the left half, the middle character (if needed), and then reversing the left half to form the right. This is fundamentally the correct approach. However, the code has a major logical flaw in handling character frequencies and a critical bug with an uninitialized variable, causing a compile error and incorrect output.

### Complexity

- **Time complexity:** O(n + 26) = O(n), where n is the length of the string (due to counting frequencies and building the string).
- **Space complexity:** O(26) = O(1) for the frequency array, plus O(n) for the output string, totaling O(n).

### vs. optimal

The optimal approach is to:
1. Count the frequency of each character.
2. Place the lexicographically smallest characters first in the left half.
3. For odd-length palindromes, use one occurrence of the smallest character with an odd count as the middle.

This code correctly follows that strategy in concept. However, the implementation is flawed - it fails to reconstruct the full palindrome properly after building the left half, omitting the symmetric right half entirely. The optimal solution would correctly mirror the left half to complete the palindrome.

### Improvements

1. **Uninitialized variable:** The variable `c` is declared but not initialized. If no character has an odd count, `c` remains uninitialized, leading to undefined behavior. Initialize it to a null character (e.g., `char c = '\0';`).

2. **Incorrect palindrome construction:** The code only builds the left half and appends the middle character, then appends the remaining characters from the frequency array directly. This produces a non-palindromic string. Instead, after building `ans` (the left half), the right half should be the reverse of `ans` (excluding the middle character if present).

3. **Redundant frequency updates:** The frequency array is modified unnecessarily when calculating `n`. The counts should be used to determine how many times to add each character to the left half, but the array itself doesn't need to be updated since it's only used for counting.

4. **Logic for odd counts:** The handling of odd counts is incomplete - it doesn't ensure that only one character gets the odd count in the middle. The code should first check if there's exactly one character with an odd frequency (required for palindrome formation) and handle it separately.

5. **Inefficient string building:** Using `ans += char` in loops is inefficient for large strings due to repeated reallocations. Use `reserve()` to pre-allocate space or build the string more efficiently.