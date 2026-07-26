---
problem: "Maximum Product of Three Numbers"
difficulty: unknown
verdict: Wrong Answer
runtime: N/A
memory: N/A
date: 2026-07-26
---

# Analysis

### Verdict summary  
The submission sorts the array and multiplies the three largest elements. This is a logical approach for positive numbers but fails because it does not consider the possibility that two large negative numbers could yield a large positive product when multiplied with the largest positive number.

### Complexity  
**Time complexity:** O(n log n) due to sorting.  
**Space complexity:** O(log n) for the sorting algorithm's recursion stack (or O(1) if using an iterative sort, but the standard sort is typically O(log n) due to recursion).

### vs. optimal  
The optimal approach recognizes that the maximum product of three numbers must be either the product of the three largest numbers or the product of the two smallest (most negative) numbers and the largest number. The optimal solution can be implemented in O(n) time by scanning the array once to find the three largest and two smallest values without fully sorting it. The submission fails to handle cases with negative numbers because it only considers the three largest elements.

### Improvements  
1. **Fix logic for negative numbers:** Instead of returning only `nums[n-1]*nums[n-2]*nums[n-3]`, compare it with `nums[0]*nums[1]*nums[n-1]` to account for negative numbers.  
2. **Avoid full sort:** Use a single pass to track the top three largest and two smallest values, reducing time complexity to O(n) and space complexity to O(1).  
3. **Use constants for clarity:** Replace magic numbers (like 3) with named constants or comments to explain the choices.