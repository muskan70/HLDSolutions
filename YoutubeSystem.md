## Youtube / Netflix System Design

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

### High Level Design
1. Video Uploading flow
2. Video Streaming flow

