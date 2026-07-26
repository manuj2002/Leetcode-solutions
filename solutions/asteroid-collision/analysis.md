---
problem: "Asteroid Collision"
difficulty: unknown
verdict: Runtime Error
runtime: N/A
memory: N/A
date: 2026-07-26
---

# Analysis

### Verdict summary
The submission attempts a stack-based simulation but contains a critical bug in handling collisions. The approach is directionally correct but fails due to accessing a popped stack element incorrectly after a same-size collision.

### Complexity
- **Time complexity:** O(n) — each asteroid is pushed once and popped at most once.
- **Space complexity:** O(n) — worst-case stack usage when no collisions occur.

### vs. optimal
The optimal solution uses a stack to simulate collisions efficiently. This submission implements the correct core logic but has a logic error: after popping the top asteroid when `abs(e) >= s.top()`, it incorrectly checks `s.top()` again after popping, leading to undefined behavior (accessing an empty stack or wrong value). The correct approach should compare sizes and handle same-size collisions without re-accessing the popped element.

### Improvements
1. **Fix collision logic:** After `s.pop()`, do not access `s.top()` again in the same condition. Instead, compare `abs(e)` with the popped value directly:
   ```cpp
   int top = s.top();
   s.pop();
   if (abs(e) == top) {
       incomingBroke = true;
       break;
   }
   ```
2. **Remove redundant condition:** The `else` block and `incomingBroke` flag can be simplified by breaking only when the incoming asteroid is destroyed. If the top asteroid is destroyed, continue the loop to check against the next stack element.
3. **Use a while-loop break conditionally:** The loop should break only when the incoming asteroid is destroyed. Otherwise, it should continue checking collisions with the remaining asteroids in the stack.
4. **Directly build the result vector:** Instead of reversing the stack at the end, use a deque or insert at the beginning to avoid the O(n) reversal step (though this is minor).