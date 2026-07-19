---
problem: "Smallest Subsequence of Distinct Characters"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 7.6 MB
date: 2026-07-19
---

# Analysis

### Verdict summary
This is not a valid solution; it hardcodes a specific output instead of implementing the algorithm. The function simply returns the string `"cbad"` for all inputs, which will fail for any test case where the expected result differs (like the provided examples). The correct approach should use a monotonic stack with frequency tracking.

### Complexity
- **Time complexity:** O(1) — It returns a constant string regardless of input size.
- **Space complexity:** O(1) — No dynamic allocation or data structures are used.

### vs. optimal
The optimal approach uses a monotonic stack to build the lexicographically smallest subsequence. Time complexity is O(n) and space complexity is O(n) (or O(1) for the frequency array since the alphabet size is fixed). This solution does not implement the algorithm at all and is incorrect.

### Improvements
1. Implement the standard monotonic stack algorithm:
   - Track last occurrence indices of each character.
   - Use a stack to build the result, ensuring lexicographic order while maintaining all distinct characters.
   - Example correct implementation:
     ```cpp
     string smallestSubsequence(string s) {
         vector<int> lastIndex(26, -1);
         vector<bool> inStack(26, false);
         stack<char> st;
         
         for (int i = 0; i < s.size(); i++) 
             lastIndex[s[i] - 'a'] = i;
         
         for (int i = 0; i < s.size(); i++) {
             if (inStack[s[i] - 'a']) continue;
             while (!st.empty() && st.top() > s[i] && lastIndex[st.top() - 'a'] > i) {
                 inStack[st.top() - 'a'] = false;
                 st.pop();
             }
             st.push(s[i]);
             inStack[s[i] - 'a'] = true;
         }
         
         string res;
         while (!st.empty()) {
             res = st.top() + res;
             st.pop();
         }
         return res;
     }
     ```

### Why the percentile is low
The code is not a functional solution—it hardcodes an output that coincidentally passes a subset of test cases (if any). Faster solutions correctly implement the greedy monotonic stack approach, which processes the string in a single pass with O(1) operations per character. This submission fails to solve the problem entirely.