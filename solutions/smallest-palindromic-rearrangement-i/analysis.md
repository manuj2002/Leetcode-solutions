---
problem: "Smallest Palindromic Rearrangement I"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-28
---

# Analysis

### Verdict summary

The submitted code attempts to use frequency counting to build the palindrome, but it contains critical flaws including an uninitialized variable and incorrect reconstruction logic. This leads to incorrect output and runtime errors.

### Complexity

**Time complexity**: O(n) - The code makes several passes through the frequency array (constant size 26) and output string (length n).  
**Space complexity**: O(n) - The output string storage dominates, plus the fixed-size frequency array.

### vs. optimal

The optimal approach for this problem is frequency counting followed by lexicographic construction:
1. Count character frequencies
2. Place characters lex smallest first on both ends
3. Handle the odd-count character (if any) in the middle

The submitted code fails because:
- Variable `c` is used uninitialized when there's no odd-frequency character
- The reconstruction logic incorrectly handles the second half by appending characters after the middle instead of mirroring the first half
- It modifies frequency counts in a way that loses information needed for proper reconstruction

### Improvements

1. **Initialize critical variables**: `c` should be initialized to -1 and only set when an odd-count character is found.

2. **Use proper palindrome construction**: Build the first half lex smallest, then mirror it for the second half:
   ```cpp
   string firstHalf = "";
   char midChar = '\0';
   
   for(int i = 0; i < 26; i++) {
       if(fre[i] % 2 == 1) {
           midChar = 'a' + i;
       }
       firstHalf += string(fre[i] / 2, 'a' + i);
   }
   
   string secondHalf = firstHalf;
   reverse(secondHalf.begin(), secondHalf.end());
   return firstHalf + (midChar ? string(1, midChar) : "") + secondHalf;
   ```

3. **Simplify frequency handling**: Don't modify the frequency array during the counting process - use the counts directly for construction.

4. **Use standard library**: `std::string` constructor with character repetition is more efficient than manual loops.