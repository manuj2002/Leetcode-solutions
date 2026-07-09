---
problem: "Two Sum"
difficulty: unknown
verdict: Accepted
runtime: 5 ms
memory: 16.2 MB
date: 2026-07-09
---

# Analysis

### Verdict summary
The submission uses a hash map to find the complement of each element, which is the correct optimal approach. However, the implementation has inefficiencies that hurt performance.

### Complexity
- **Time Complexity:** O(n) - Each element is processed once (O(n)) and hash map operations are average O(1).
- **Space Complexity:** O(n) - The unordered_map stores up to n elements.

### vs. optimal
The optimal solution for this problem uses a single-pass hash table (exactly as implemented here conceptually). While the approach is optimal in theory, the implementation details make it slower than other O(n) solutions.

### Improvements
1. **Use `unordered_map::find` instead of checking count then accessing:** The code does `if (mp.count(...))` followed by `mp[complement]`. This performs two lookups when one suffices. Replace with:
   ```cpp
   auto it = mp.find(complement);
   if (it != mp.end()) return {it->second, i};
   ```
2. **Reserve map capacity:** The constraints state up to 10⁴ elements. Adding `mp.reserve(nums.size());` before the loop avoids rehashing overhead.
3. **Avoid unnecessary `pair`/`vector` construction:** Returning `{it->second, i}` directly is more efficient than constructing a vector separately.

### Why the percentile is low
The performance percentile is low because:
- The double lookup (`count` followed by `[]`) adds significant overhead in a tight loop.
- Lack of map capacity reservation causes unnecessary rehashing during insertion.
- Faster solutions use single-lookup patterns and often pre-allocate the hash table’s capacity.