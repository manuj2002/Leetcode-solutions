---
problem: "Two Sum"
difficulty: unknown
verdict: Accepted
runtime: 4 ms
memory: 16.3 MB
date: 2026-07-09
---

# Analysis

### Verdict Summary
The submitted solution uses a hash map to store the last occurrence index of each number. It then performs a second pass to find the complement of each number, checking that the index differs. This general approach is correct for the two-sum problem, but the implementation has efficiency and correctness issues.

### Complexity
- **Time Complexity:** O(n) in the average case, but O(n²) worst-case if many hash collisions occur (though unlikely in practice with `std::map`'s O(log n) operations).
- **Space Complexity:** O(n) for storing the hash map.

### vs. Optimal
The optimal approach for two-sum uses a single-pass hash map: while iterating, check if the complement exists in the map. If not, insert the current number. This yields O(n) time and O(n) space with better constant factors. The submission uses two passes and a less efficient `std::map` instead of `std::unordered_map`, making it suboptimal.

### Improvements
1. **Use `std::unordered_map` instead of `std::map`:** `std::map` has O(log n) insert/lookup, while `std::unordered_map` has O(1) average-case operations.
2. **Single-pass approach:** Avoid the second pass by checking for the complement during the initial iteration. This also handles cases where the complement might appear later without needing to enforce `x[y] != i` explicitly.
3. **Avoid redundant vector allocation:** Initialize the result vector only when the solution is found, and return it directly instead of using a temporary.
4. **Use `find()` instead of `contains()` for broader compiler compatibility (C++20 required for `contains`).**

Improved code:
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> seen;
        for (int i = 0; i < nums.size(); ++i) {
            int complement = target - nums[i];
            if (seen.find(complement) != seen.end()) {
                return {seen[complement], i};
            }
            seen[nums[i]] = i;
        }
        return {};
    }
};
```

### Why the Percentile Is Low
The runtime percentile is low because:
- **Two-pass logic:** The solution iterates twice over the array, increasing constant factors.
- **Inefficient data structure:** `std::map` is slower than `std::unordered_map` for this use case.
- **Late complement check:** The single-pass approach finds the solution earlier in many cases, reducing average runtime. Faster submissions use these optimizations.