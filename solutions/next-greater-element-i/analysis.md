---
problem: "Next Greater Element I"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.6 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
This solution correctly uses a monotonic stack to precompute the next greater element for every value in `nums2`, storing results in a hash map for quick lookup in `nums1`. This is the optimal approach for the problem.

### Complexity
- **Time complexity:** O(m + n) where m = nums1.size(), n = nums2.size()
- **Space complexity:** O(n) for the stack and hash map

### vs. optimal
Your solution is optimal. It uses the standard approach: process `nums2` once with a monotonic stack to build a mapping from each element to its next greater element, then query this mapping for each element in `nums1`. The complexity matches the theoretical best possible.

### Improvements
1. **Replace `m.contains()` with direct lookup:** The condition `if (m.contains(nums1[i]) > 0)` is inefficient. Since `m[nums1[i]]` defaults to 0 for missing keys (but 0 is a valid value), this creates ambiguity. Better to check `if (m.count(nums1[i]))` or leverage the fact that unset keys will return 0, but since all values are ≥ 0 and -1 indicates "not found", initialize the map values to -1 instead.

2. **Initialize map values to -1 explicitly:** The current code only inserts entries when a next greater element exists. This means missing keys in the map default to 0 (via `operator[]`), which is incorrect if 0 is a valid answer. Better to preprocess all elements in `nums2` to have -1 as default.

Revised code:
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> m;
        stack<int> s;
        
        for (auto e : nums2) {
            while (!s.empty() && s.top() < e) {
                m[s.top()] = e;
                s.pop();
            }
            s.push(e);
        }
        
        // Set remaining elements in stack to -1
        while (!s.empty()) {
            m[s.top()] = -1;
            s.pop();
        }
        
        vector<int> ans;
        for (auto num : nums1) {
            ans.push_back(m[num]);
        }
        return ans;
    }
};
```

### Why the percentile is low
Despite O(m+n) complexity, the runtime percentile varies due to:
- **Map initialization overhead:** Small input sizes (≤1000) make constant factors significant. Some solutions use vectors instead of hash maps for O(1) lookup when value ranges are small.
- **Memory usage:** The stack and hash map allocation may be optimized in faster solutions by using a pre-sized vector for the next greater element mapping instead of a hash map, if the value range is known to be small (though here 0 ≤ nums2[i] ≤ 10^4 makes this less practical).