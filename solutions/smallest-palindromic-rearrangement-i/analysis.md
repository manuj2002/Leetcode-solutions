---
problem: "Smallest Palindromic Rearrangement I"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 7.9 MB
date: 2026-07-28
---

# Analysis

### Verdict summary
The submitted approach attempts to build the smallest palindrome by filling the first half in ascending order, handling an odd-count character in the middle, and then mirroring the first half. This is fundamentally correct for the problem, but the implementation has logical flaws that would cause incorrect outputs for some inputs, despite being marked "Accepted".

### Complexity
- **Time complexity**: O(n) — The code makes three passes over the frequency array (size 26) and constructs the string linearly.  
- **Space complexity**: O(n) — For the output string; the frequency array uses O(1) extra space.

### vs. optimal
The optimal approach for this problem is to:  
1. Count character frequencies.  
2. For the smallest palindrome, place characters in ascending order in the first half.  
3. If there’s an odd-frequency character, it must be the middle character (and there can be only one, since the input is already a palindrome).  
4. Mirror the first half to form the palindrome.  

The submitted code is **not optimal** because it fails to correctly handle cases where the odd-count character is not the lexicographically smallest possible middle character. Specifically, it always uses the last odd-count character encountered in the loop as the middle, which may not be the smallest valid choice. The input being palindromic guarantees at most one odd-count character, so the loop should identify that character correctly, but the code’s logic for selecting `c` is flawed—it overwrites `c` with every odd-count character, so only the last one remains. This happens to work if there’s exactly one odd-count char (as in the examples), but the problem’s constraints ensure the input is a palindrome, so there is exactly one odd-count character (if length is odd) or none (if even). Thus, the code accidentally works for all valid inputs, but the logic is still error-prone.

### Improvements
1. **Fix odd-character handling**: Instead of updating `c` for every odd-count character, break after finding the first one (since a palindrome can have at most one odd-count character). Even better: during the first half construction, track if an odd-count character exists and use the smallest such character for the middle.  
2. **Simplify frequency management**: The variables `n` and fre[i] updates are redundant. Compute half-counts directly without modifying the frequency array mid-loop.  
3. **Use string building efficiently**: Pre-allocate the string size to avoid repeated reallocations, especially since the output length is known.  
4. **Code clarity**: The loop for the second half can be written more clearly by reversing the first half (excluding the middle) instead of manually iterating backward.

Example corrected snippet:
```cpp
string smallestPalindrome(string s) {
    vector<int> freq(26, 0);
    for (char c : s) freq[c - 'a']++;
    
    string half;
    char odd_char = 0;
    for (int i = 0; i < 26; i++) {
        if (freq[i] % 2) odd_char = 'a' + i; // There will be at most one
        half += string(freq[i] / 2, 'a' + i);
    }
    
    string second_half(half.rbegin(), half.rend());
    return half + (odd_char ? string(1, odd_char) : "") + second_half;
}
```

### Why the percentile is low
The runtime percentile is low because the solution uses a suboptimal string-building approach (appending characters in loops with `+=`), which may cause reallocations. Faster solutions pre-allocate the result string and assign characters by index, avoiding expensive append operations. Additionally, the logic for handling the middle character is inefficient—it does unnecessary work by processing all characters twice in separate loops instead of constructing the palindrome in one pass.