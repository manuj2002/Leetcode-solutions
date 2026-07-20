---
problem: "Combination Sum"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-20
---

# Analysis

### Verdict summary
The code uses a backtracking approach with recursion to generate combinations. This is a correct strategy for the problem, but the implementation has a syntax error and inefficiencies in handling the combinations.

### Complexity
**Time Complexity:** O(N^T) — In the worst case, the algorithm explores all possible combinations where T is the target (each element can be taken up to T times).  
**Space Complexity:** O(T) — The recursion depth is bounded by T (since at most T elements are added to a combination), but the space for storing all combinations is output-dependent.

### vs. optimal
The optimal approach for this problem is backtracking with pruning. The code attempts this but has a key inefficiency: it creates a new vector `t` for every candidate at every recursion level, leading to unnecessary copying. The standard optimal approach uses a single vector that is modified in-place (pushed and popped) to avoid copying overhead. Additionally, sorting the array and breaking early when the candidate exceeds the remaining target is common for pruning.

### Improvements
1. **Fix the syntax error:** On line 21, change `vetcor` to `vector`.
2. **Avoid vector copies:** Instead of creating copies (`vector<int> t = v`), use a single vector passed by reference and backtrack by popping after recursion. This reduces both time and space overhead.
3. **Add pruning:** Sort the candidates first and break early if the current candidate exceeds the remaining target. This avoids unnecessary recursive calls.
4. **Use iterative inclusion:** The current code uses a loop to add multiple copies of the same candidate, but the recursion should start at the same index (not i+1) when including the same candidate again, which is more efficient and idiomatic.

Example improved code structure:
```cpp
void combo(int start, vector<int>& candidates, int target, vector<int>& current, vector<vector<int>>& ans) {
    if (target == 0) {
        ans.push_back(current);
        return;
    }
    for (int i = start; i < candidates.size(); i++) {
        if (candidates[i] > target) break;
        current.push_back(candidates[i]);
        combo(i, candidates, target - candidates[i], current, ans);
        current.pop_back();
    }
}
```