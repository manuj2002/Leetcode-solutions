---
problem: "Rank Transform of an Array"
difficulty: unknown
verdict: Accepted
runtime: 19 ms
memory: 39.9 MB
date: 2026-07-12
---

# Analysis

### Verdict summary
The solution correctly sorts pairs of (value, index) to assign ranks. It is an optimal approach that efficiently handles duplicate values by comparing adjacent elements during the rank assignment.

### Complexity
- **Time complexity:** O(n log n) due to sorting the array of size n.  
- **Space complexity:** O(n) for storing the pairs and the result vector.

### vs. optimal
This **is the optimal solution** for the problem. The standard approach requires sorting the values while tracking original indices, then assigning ranks in a single pass. The solution achieves the best possible time complexity for this problem, as sorting is necessary to determine relative ordering.

### Improvements
1. **Avoid redundant pairs vector:** Instead of creating a vector of pairs, use an index array and sort it using a custom comparator that references the original `arr`.  
   ```cpp
   vector<int> idx(n);
   iota(idx.begin(), idx.end(), 0);
   sort(idx.begin(), idx.end(), [&](int a, int b) { return arr[a] < arr[b]; });
   ```
   This avoids storing duplicate values and reduces memory overhead.

2. **Simplify rank assignment logic:** Initialize `rank = 0` and increment only when encountering a new value. The current logic with `j` is correct but could be clearer by incrementing after assigning the rank for the first occurrence of a value.

3. **Use emplace_back for efficiency:** When building the pairs vector, `emplace_back(arr[i], i)` avoids creating temporary pairs and is more efficient than `push_back({arr[i], i})`.

4. **Preallocate and avoid resizing:** The `ans` vector is correctly sized initially, but ensure no further reallocations occur by using `reserve` for the pairs vector if the size is known.