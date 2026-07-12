---
problem: "Rank Transform of an Array"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-12
---

# Analysis

### Verdict summary
The approach intends to sort pairs of (index, value) to assign ranks, which is a valid way to handle the problem. However, the implementation has multiple syntax errors and logical flaws that cause it to be incorrect even if compiled.

### Complexity
Assuming the syntax errors are fixed and the algorithm is corrected, time complexity would be O(n log n) due to sorting, and space complexity would be O(n) for storing pairs and the result.

### vs. optimal
The optimal approach is: copy and deduplicate `arr` → sort unique values → map each original value to its index in sorted unique values + 1. Complexity O(n log n) time, O(n) space. Your current approach is unnecessarily complicated because you're storing indices and trying to assign ranks in a single sorted pass while incorrectly handling duplicate values and index retrieval.

### Improvements
1. **Compilation error**: On line 5, `vector<pair<int,int>>>ar;` has an extra `>` — should be `vector<pair<int,int>> ar;`.

2. **Incorrect sorting**: The current `sort` compares pairs by index first (since `pair`’s default comparison uses `first`), but you need to sort by the element value. Use `sort(ar.begin(), ar.end(), [](auto& a, auto& b){ return a.second < b.second; });`.

3. **Rank assignment logic**: The rank `j` incrementation is backwards — when values are equal, rank should *not* increase. Also, you refer to `ar.first` which doesn't exist; should be `ans[ar[i].first] = j;`. A corrected version should track the previous value and increment rank only when the current value is larger, not equal.

4. **Redundant storage**: Instead of storing (index, value) for all elements, you can simply sort a copy of `arr` and build a hash map from value to rank, then map the original array. This is cleaner, reduces bugs, and matches the optimal method described above.