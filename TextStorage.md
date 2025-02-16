## Text Storage System Design
Note: Here in this design lot of points are discussed similar to url shortner

### Functional Requirements
1. User should be able to store a huge text into system. 
2. System should return a URL which allows user to see stored text, share among other users.
3. Only users with authentication can access the shared Url.

### Non Functional Requirements
1. Scale: There can be around 1 trillion links to store.
2. Reads to Writes Ratio => 10:1, Reads>> Writes
3. Highly Available: System should be highly available otherwise user will not be able to access a link.

### Back of Envelope Estimation:
- Writes: 100 million writes per day
- Writes per second = 100 million /24*360 = 1160
- Reads per second = 10*1160 = 11600
- Average Text Length = 10 KB
- Total storage = 1 Trillion * 10 KB = 10 PB

### API Design
1. POST /api/v1/data/url
- Request Parameter {data: text}
- return URL

2. GET /api/v1/url
- Returns text stored corresponding to that url

### Database Design
1. For storing huge files in GB, S3 object storage can be used.
2. **Schema**: UniqueID, urlToStoredText, shortURL, userId, createTime, expireTime, clicks
3. Single Leader Replication to avoid collisions 
4. Data Partitioning based on range of shortURL, hashes support even partitioning
5. B+ tree index based on faster reads
6. CDNs can be used for faster reads

### Write Path
> write first in S3, then add entry in database to avoid invalid entries.
> Popolating CDN: write through CDN can help here as cache miss is quite expensive here due to huge file size.
