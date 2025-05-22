## Youtube System Design

### Functional Requirements
1. User should be able to upload a video.
2. Viewers can watch, like and comment on videos.
3. Videos can be searched by name.

### Non Functional Requirements
1. System should support different types of video quality and client devices both at uploading videos and watching them.
2. High availability, scalability, and reliability requirements
3. Users watching videos is much more compared to uploading videos -> Reads >> Writes

### Back of Envelope Estimation
1. Daily active users = 5 million with each user watching 5 videos per day on average and 10%of users upload 1 video per day.
2. average video size = 300MB
3. Daily storage = 5 million * 10% * 300 MB = 150TB

### API Design
1. Video Uploading flow
- POST /api/v1/video/upload
- upload video metadata and chunks of video

2. Video Streaming flow
- GET /api/v1/video/get/details
- return video metadata

### Database Design
1. SubscribersDB
- Schema -> {userId, subscribedTo}
- Graph Database : two types of queries here, one is getSubscribers, other is getSubscribedTo : Shard, Index and partition on userId

2. VideoDetailsDB
- Schema -> {userId, videosId, timestamp, videoName, description}
- MySQL: Shard by userId to get all videos of a user on single node
- Partitioned by userId, sort by userId + videoId + timestamp

3. UserDB
- Schema -> { userId, name, email, passwordHash ...}
- MySQL: partioned + index on userId

4. VideoCommentsDB
- Schema -> {channelId, videoId, timestamp, userId, comment}
- Cassandra: Partioned on channelId + videoId + timestamp

5. VideoChunkMetadataDB
- Schema -> {videoId, encoding, resolution, chunkOrder, hash, URL}
- MySQL: partition on videoId to get all chunks of video on same node
- sort by videoId, encoding, resolution, chunkOrder

6. VideoChunk Storage - S3: less expensive, option2 can be HDFS: offers better data locality but expensive



