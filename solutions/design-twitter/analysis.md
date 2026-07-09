---
problem: "Design Twitter"
difficulty: unknown
verdict: Accepted
runtime: 0 ms
memory: 10.7 MB
date: 2026-07-09
---

# Analysis

### Verdict summary
This implementation uses an unconventional approach that redundantly stores tweets in multiple stacks based on follower relationships. While it passes the given test cases due to the low constraints, the design is fundamentally inefficient and does not scale well. The approach is not optimal for real-world scenarios.

### Complexity
- **Time Complexity**: 
  - `postTweet`: O(n) where n is the number of users (501 in this case)
  - `getNewsFeed`: O(k) where k is the number of tweets popped (up to 10), but with expensive stack reconstruction.
  - `follow`/`unfollow`: O(1)
- **Space Complexity**: O(n² + t) where n=501 users and t is the total number of tweets (each tweet is stored in multiple stacks).

### vs. optimal
The optimal solution uses:
- A global timestamp to track tweet recency
- Separate storage for user tweets and follower graphs
- A max-heap to merge recent tweets from followed users

**Optimal complexities**:
- `postTweet`: O(1)
- `getNewsFeed`: O(f log f) where f is number of followed users
- `follow`/`unfollow`: O(1)

Your approach differs by redundantly copying tweets to follower stacks, causing O(n) `postTweet` and inefficient stack manipulation in `getNewsFeed`.

### Improvements
1. **Remove redundant tweet storage**: Instead of copying tweets to each follower's stack, maintain a global tweet list with timestamps and merge feeds on-demand.
2. **Use proper data structures**: Replace the 501×501 follower matrix with `unordered_map<int, unordered_set<int>>` for sparse storage.
3. **Avoid stack reconstruction**: The current `getNewsFeed` destructively pops and rebuilds the stack - use non-destructive iteration.
4. **Add timestamp tracking**: Without timestamps, you cannot properly order tweets when merging multiple users' feeds.

### Why the percentile is low
The low runtime percentile is deceptive - it's only "fast" because the constraints are small (501 users). The solution would fail with realistic user counts due to O(n) `postTweet` operations and excessive memory usage. Faster solutions use O(1) posting and merge feeds efficiently using heaps rather than storing redundant copies.