---
problem: "Design Twitter"
difficulty: unknown
verdict: Accepted
runtime: 2 ms
memory: 10.5 MB
date: 2026-07-09
---

# Analysis

### Verdict Summary
This implementation uses a brute-force approach where each user has their own stack of tweets, and when posting, the tweet is pushed to all followers' stacks. While accepted, this is inefficient and does not scale well with the constraints. The design is fundamentally flawed for a real-time system.

### Complexity
- **Time Complexity**:  
  - `postTweet`: O(n) where n = 500 (user count) — iterates over all users to push to followers.  
  - `getNewsFeed`: O(n) for popping and repushing up to 10 tweets.  
  - `follow`/`unfollow`: O(1) for matrix updates.  
- **Space Complexity**: O(n²) for the follower matrix + O(n * t) for tweet storage (where t is total tweets per user).

### vs. Optimal
The optimal approach uses:
- A global timestamp to order tweets.
- A hash map storing each user's tweets as a list (linked list preferred for merging).
- A hash map storing each user's followees.
- `getNewsFeed` merges tweets from followees using a max-heap (priority queue) of size ≤ 10.  
**Complexity**:  
- `postTweet`: O(1)  
- `getNewsFeed`: O(f * log k) where f is followee count, k=10  
- `follow`/`unfollow`: O(1)  
This solution is optimal and scales to large inputs.

### Improvements
1. **Replace stack with timestamped tweets**: Use a global counter and store `(time, tweetId, userId)` for ordering.  
2. **Use adjacency list for followers**: Replace 500x500 matrix with `unordered_map<int, unordered_set<int>>` to save space.  
3. **Merge feeds efficiently**: In `getNewsFeed`, use a heap to merge recent tweets from followees instead of storing all tweets per user.  
4. **Fix unfollow bug**: Line 43 incorrectly sets `follower[followeeId][followeeId]=0`; it should be `follower[followeeId][followerId]=0`.  
5. **Avoid redundant storage**: Don’t push tweets to all followers on `postTweet`; instead, aggregate during `getNewsFeed`.

### Why the Percentile Is Low
The runtime/memory percentiles are low because:  
- `postTweet` does O(n) work unnecessarily, while optimal solutions do O(1).  
- Space usage is excessive due to the matrix and redundant tweet storage.  
- The feed retrieval is inefficiently implemented with stack popping/repushing instead of a heap merge.  
Faster solutions aggregate tweets on-demand using heaps and avoid pre-distributing tweets to followers.