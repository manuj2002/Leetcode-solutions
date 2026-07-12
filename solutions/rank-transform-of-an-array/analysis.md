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
The approach attempts to sort pairs of indices and values to assign ranks, but it has a critical error in accessing the sorted vector. The idea is correct but the implementation is flawed.

### Complexity
Time: O(n log n) from sorting. Space: O(n) for storing pairs.

### vs. optimal
The optimal approach sorts unique values to assign ranks, using a hash map for lookup. This code's method is conceptually correct but fails due to incorrect indexing and rank assignment logic.

### Improvements
1. **Line 16**: `ar.first` should be `ar[i].first` to access the original index.
2. **Rank assignment logic**: The current condition `if(i-1>=0 && ar[i-1].second==ar[i].second)` increments `j` incorrectly. Instead, compare with the previous element and only increment when values differ.
3. **Sorting**: Sort by value instead of index? Actually, the pairs are created as `(index, value)`, but then sorted by index (default pair comparison), which is wrong. Should sort by value: use `sort(ar.begin(), ar.end(), [](auto& a, auto& b) { return a.second < b.second; });`.
4. **Efficiency**: Avoid storing indices; instead, sort the array and use a map for ranks, then reconstruct. This reduces overhead.

Revised code:
```cpp
vector<int> arrayRankTransform(vector<int>& arr) {
    vector<int> sorted = arr;
    sort(sorted.begin(), sorted.end());
    unordered_map<int, int> rank;
    int r = 1;
    for (int num : sorted) {
        if (rank.find(num) == rank.end()) {
            rank[num] = r++;
        }
    }
    for (int& num : arr) {
        num = rank[num];
    }
    return arr;
}
```