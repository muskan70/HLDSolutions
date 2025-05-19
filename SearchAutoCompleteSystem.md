## Design Google Search Bar Auto Complete System
Google search bar suggesting full search terms for single or multiple words

### Functional Requirements
1. System should be able to load top k suggestions relevant to search term/input text.
2. Results returned by system should be sorted by popularity.
3. Top terms list should be updated at least once per day.

### Non Functional Requirements
1. **Fast response time** i.e Minimal Read Latency : As a user types a search query, autocomplete suggestions must show up fast enough
2. **Relevant**: Autocomplete suggestions should be relevant to the search term.
3. **Sorted**: Results returned by the system must be sorted by popularity or other ranking models.
4. **Scalable**: The system can handle high traffic volume, around 10 million DAUs.
5. **Highly available**: The system should remain available and accessible when part of the system is offline, slows down, or experiences network errors.

### Back of Envelope Estimation
- With 10 million daily active users, average user performing 10 queries per day -> 20 bytes per query ~ 1 billion unique search queries
- ~24,000 query per second (QPS) = 10,000,000 users * 10 queries / day * 20 characters / 24 hours / 3600 seconds.
- Peak QPS = QPS * 2 = ~48,000
- There are 1 billion unique search queries per day with average length of 20 characters (around 4 words) -> to store counts per search term: 1 billion * 4 bytes(int) = 4 GB
- To store prefix for each search term ->(20*1 billion) = 20 billion entries 
- 20 billion * (10 search terms * 20 characters per term + prefix) = 20 billion * 200 bytes per search query = 4 TB -> can't be stored on client's device nor on single server

### Google Search bar Auto Suggestion High Level Design 
1. **Data Gathering Service**
2. **Query Service**