## Distributed Key Value Store System Design

### Functional Requirements
1. Key should be unique.
2. Values can be string, list or object.
3. System should support put (key, value)pair and get(key)

### Non Functional Requirements
1. Key Value pair size should be less than 10 KB.
2. High Availability: System should respond even during failures.
3. Highly Scalable: System should support large data set.
4. Low Latency: System should return value corresponding to given key with minimum latency.

### Points to discuss
1. Data Partitioning: using consistent hashing to allow automatic scaling and heteroginity.
2. Data Replication: across multiple data centres 
3. Consistency: eventual consistency maintained using version vectors
4. Handling Failures: Using gossip protocol to detect server/node is down, Tempory failure is handled through sloppy quorum -> hinted handoff, parmanent failures are handled via merkel tree

### API Design
1. GET /api/getValye?key=?
2. PUT /api/putKey  {key:xxx, value:xxx}

### Database Design
1. Memory Cache : to store data during writes,and flush to database once cache is full
2. Sorted string Table (SS Tables): It is used for persistent key value storage in nodes
3. Bloom Filter : It is used to search which SS table contains the key.

[Design](./images/KeyValueStoreDesign.png)




