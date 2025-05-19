## Design Typeahead or Search Auto Complete System
**Simple Usecase**: Typeahead suggestion by Google keyboard, samsung keyboard used in mobiles during typing : single word auto suggestion corresponding to text input by keyboard, algorithmic problem solved by trie search

**Complex Usecase**: Google search bar suggesting full search terms for single or multiple words

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
1. For typeahead suggestion to suggest top 3 words:
> - There are around 200k words in english with average length of 5 characters -> to store counts per search term: 200k * 4 bytes = 800k
> - To store prefix for each word ->(5*200k) = 1 million entries
> - 1 million * (5 bytes for word  + 15 bytes for suggestions) = 20 MB -> can be easily stored on client's device

2. For Google search bar to suggest top 10 search terms:
> - With 10 million daily active users, average user performing 10 queries per day -> 20 bytes per query ~ 1 billion unique search queries
> - ~24,000 query per second (QPS) = 10,000,000 users * 10 queries / day * 20 characters / 24 hours / 3600 seconds.
> - Peak QPS = QPS * 2 = ~48,000
> - There are 1 billion unique search queries per day with average length of 20 characters (around 4 words) -> to store counts per search term: 1 billion * 4 bytes(int) = 4 GB
> - To store prefix for each search term ->(20*1 billion) = 20 billion entries 
> - 20 billion * (10 search terms * 20 characters per term + prefix) = 20 billion * 200 bytes per search query = 4 TB -> can't be stored on client's device nor on single server

### Typeahead Suggestion Design Approach
#### Local Non Caching Approach: 
- Store sorted/ordered hashmap of words with counts,
- when user types a word, query hashmap to get all words with same prefix as query using binary search
- then find top k suggestions using min heap of size k
- very slow computation : Search Complexity -> O(nlogk)

#### Local Caching Approach:
- Store sorted map of prefix to its top k suggestions
- Search Complexity -> O(len(prefix))
- Memory footprint wrt to each term -> O(n^2) to store all prefixes

#### Trie Approach:
- Store Trie of words such that each node stores top k suggestions of that prefix
- Traversing the prefix via trie takes O(1) time i.e when user type next character, getting suggestion takes O(1) time so jumping from one search term to another takes constant time.
- Memory footprint wrt to each term -> O(n)
- Using dictionary compression for popular words, creating map from short type(2 bytes) reduces storage of suggested words at each node
- Trie can be stored inmemory for faster reads
- **Problem with writes/updates**:
> - Updating a trie in real time will be quite complex because if a single write turns a node(word) into popular ones, it needs to be propagated all the way up the tree.<br>
> - Also all writes needs to be refected on every client device, so better to replace the trie with new one with all writes/updates once per day.<br>
> - Steps -> When a user clicks a word, upload that action to HDFS and then once a day run a spark job to compute top suggestions for each prefix, once done create a new trie and send it to all client devices.

### Google Search bar Auto Suggestion High Level Design 
1. **Data Gathering Service**
2. **Query Service**