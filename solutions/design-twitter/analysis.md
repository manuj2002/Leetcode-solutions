---
problem: "Design Twitter"
difficulty: unknown
verdict: Compile Error
runtime: N/A
memory: N/A
date: 2026-07-09
---

# Analysis

### Verdict summary
The attempted approach uses a 2D adjacency matrix for follower relationships and a map of stacks to store tweets, which is fundamentally flawed. The `getNewsFeed` method fails to return the required vector, causing a compile error. Even if fixed, the design does not scale efficiently.

### Complexity
- **Time Complexity**: 
  - `postTweet`: O(U) where U=501 (fixed constant) due to fanout to all possible followers
  - `getNewsFeed`: O(10) per tweet retrieval but restores the stack in O(10) using a queue
  - `follow`/`unfollow`: O(1)
- **Space Complexity**: O(U² + T) for the 501×501 matrix and map of stacks storing up to T tweets.

### vs. optimal
The optimal solution uses:
- A hash map storing each user's tweets as a linked list (or deque) with timestamps
- A hash map storing each user's followees as a set
- A max-heap (priority queue) to merge the most recent tweets from followees in `getNewsFeed`

**Complexity**: 
- `postTweet`: O(1)
- `getNewsFeed`: O(F log F) where F is number of followees (heap operations)
- `follow`/`unfollow`: O(1)
- Space: O(U + T)

This submission differs by using inefficient fanout on every post and a stack that requires full reconstruction for news feed queries.

### Improvements
1. **Missing return statement**: `getNewsFeed` must return `ans` (line 36).
2. **Wrong unfollow logic**: Line 49 should set `follower[followeeId][followerId] = 0`.
3. **Inefficient fanout**: Storing duplicate tweets for all followers wastes space. Instead, store tweets per user and merge during `getNewsFeed`.
4. **Non-scalable design**: The fixed 501×501 matrix limits user IDs and wastes memory. Use `unordered_map<int, unordered_set<int>>` for follow relationships.
5. **Stack misuse**: Stacks don't support efficient filtering. Use a list/deque with timestamps and a heap for merge k sorted lists.