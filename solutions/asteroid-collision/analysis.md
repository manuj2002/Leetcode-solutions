---
problem: "Asteroid Collision"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 8.4 MB
date: 2026-07-26
---

# Analysis

### Verdict summary
The solution correctly uses a stack to simulate collisions between moving asteroids. The approach handles right-moving (positive) and left-moving (negative) collisions efficiently and is the standard optimal method.  

### Complexity
**Time:** O(n). Each asteroid is pushed onto the stack at most once and popped at most once.  
**Space:** O(n). Stack usage can grow up to the size of the input.  

### vs. optimal  
This **is** the known optimal approach for this problem. The typical solution uses a stack and processes each asteroid once, with potential collisions handled in the stack loop—exactly as done here.  

### Improvements  
1. **Handling the case when `abs(e) == s.top()`:** Currently `incomingBroke=true` and the top is kept, but the top should be popped and neither asteroid retained. Code should be changed to `if(abs(e) > s.top()) s.pop(); else if(abs(e) == s.top()) { s.pop(); incomingBroke = true; break; }` to correctly destroy both.
2. **Unnecessary `incomingBroke` variable:** Simplify by using explicit break/pop logic without a separate boolean flag by comparing magnitudes inside the loop.
3. **Build answer in reverse directly:** Avoid separate reversal by using a vector as the stack itself, then returning it.  

### Why the percentile is low  
Runtime percentiles are often influenced by small constant factors. Faster solutions may:  
- Use an in-place vector as the stack, avoiding separate `stack<int>` and reducing memory allocations.  
- Process equality case more efficiently without extra flags.  
- Iterate from front to back and build result directly without reversal (using vector's `push_back` and `pop_back` as stack ops).