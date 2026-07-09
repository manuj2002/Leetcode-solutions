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
Your approach incorrectly aggregates tweets into each follower’s stack in `postTweet`, which does not scale and loses ordering after pops in `getNewsFeed`. The core idea is flawed—you need a global timestamp and merge recent tweets from followed users.

### Complexity
- **Time**: `postTweet` O(500) per call (≈ O(1) but constant 500). `getNewsFeed` removes all tweets from a user’s stack each time, copying them via a queue for restoration, making it O(total tweets of followed users) per call.
- **Space**: `news[user]` stack duplicates each tweet for all followers—O(N * F) where N is tweets, F is followers. Exceeds reasonable memory.

### vs. optimal
Optimal design uses:
- A global incremental timestamp.
- For each user: a list/vector of their own tweets (with timestamp).
- Follow relationships stored as a hash set per user.
- `getNewsFeed`: collect the most recent 10 from the user + all followees by merging using a max-heap of iterators/linked-list pointers (k-way merge).  
Complexity: `postTweet` O(1), `follow/unfollow` O(1), `getNewsFeed` O(10 log F) where F is number of followed users.

Your approach differs by duplicating tweets into follower stacks and scanning entire fixed-size matrices.

### Improvements
1. **Fix `vecto` typo** (compile error): Line 2 should be `vector<vector<int>> follower;`.
2. **Store tweets per user with timestamps**: Replace the map of stacks with `unordered_map<int, vector<pair<time_t, int>>> userTweets`, adding a global time counter.
3. **Separate follow graph correctly**: Use `unordered_map<int, unordered_set<int>> following`. Your current matrix transposition and 500 limit are wasteful and incorrectly oriented.
4. **Use max-heap for getNewsFeed**: Collect head tweets from user + followees, push (timestamp, tweetId, userId, index) into heap, pop top 10.
5. **Missing semicolon** in `follow` method (line 48).
6. **Remove redundant copying via queue**: In your `getNewsFeed`, you empty stack, push to queue, then restore. This is unnecessary with proper merge algorithm.