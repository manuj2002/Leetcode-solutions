---
problem: "Two Sum"
difficulty: unknown
verdict: Accepted
runtime: 6 ms
memory: 16.3 MB
date: 2026-07-09
---

# Analysis

### Verdict summary
The submission uses a hash map to store each element and its index, checking for the complement (target - current number) in the map. This is the optimal approach for the problem, achieving O(n) time complexity.

### Complexity
**Time:** O(n) — Each element is processed once, with O(1) operations per element for map insertion and lookup.  
**Space:** O(n) — The hash map stores up to n elements in the worst case.

### vs. optimal
This solution is optimal. The known optimal approach uses a hash table to store seen numbers and their indices, allowing for O(1) complement checks. This matches the submission's strategy exactly.

### Improvements
1. **Use `unordered_map` instead of `map`:** The code uses `std::map`, which has O(log n) insertion and lookup per operation. Switching to `std::unordered_map` (average O(1) per operation) would improve constant factors significantly.
2. **Preallocate memory:** Reserve space in the hash map upfront (e.g., `reserve(nums.size())`) to avoid rehashing and reduce dynamic allocations.
3. **Avoid redundant lookup:** The code performs two lookups: one to check if the complement exists and another to retrieve its index. Use `find` and store the iterator to avoid this.

### Why the percentile is low
The runtime percentile is low because the code uses `std::map` (red-black tree) instead of `std::unordered_map` (hash table). The tree-based map has O(log n) per operation, making the overall complexity O(n log n) in practice, despite the theoretical O(n) with a hash table. The memory percentile is low due to lack of preallocation and the higher overhead of tree nodes compared to hash table buckets.