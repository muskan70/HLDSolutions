## News Feed System Design

### Functional Requirements
1. When a user opens app,he can publish a feed as well as see feeds of other users he follows.
2. User can like, comment on those posts.
3. These feeds can be arranged according to their popularity in reverse cronological order.
4. Instead of loading all feeds in one go, feeds can be loaded using pagination.
5. Feed can consist of media files like images, videos.

### Non Functional Requirements
1.Scale of the system:
- Distribution of Users: We can consider around 1 billion users with 50% daily active users, out of which 10% writes post, others are present to read the feeds => Reads >> Writes
2. Latency: Feeds should be fetched with minimal latency as low as 1-2 s.
3. Freshness of Feeds : We can refresh feeds within every Hour.
4. System should be highly reliable if not Users will loose interest in using the app.

### API Design
1. POST /api/feed/v1/post?{userId}
- Request Body:{content:xxxxx}
2. GET /api/feed/v1/get?{userId}

### Database Design
1. Regular Feed:
- Schema: FollowerId, PosterId, PostId, popularityIndex
- NoSQL, Partioned by FollowerId

2. Popular Feed:
- Schema: PosterId,PostId, popularityIndex
- NoSQL, Partitioned by PosterId
