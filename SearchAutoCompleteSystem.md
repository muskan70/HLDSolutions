## Design Typeahead or Search Auto Complete System
**Simple Usecase**: Typeahead suggestion by Google keyboard, samsung keyboard used in mobiles during typing
- single word auto suggestion corresponding to text input by keyboard. 
- algorithmic problem solved by trie search

**Complex Usecase**: Google search bar suggesting full search terms for single or multiple words

### Functional Requirements
1. System should be able to load top k suggestions relevant to search term/input text.
2. Results returned by system should be sorted by popularity.
3. Top terms list should be updated at least once per day.

### Non Functional Requirements
1. Minimal Read Latency

### Back of Envelope Estimation
1. For typeahead suggestion to suggest top 3 words:
- There are around 200k words in english with average length of 5 characters -> to store counts per search term: 200k * 4 bytes = 800k
- To store prefix for each word ->(5*200k) = 1 million entries
- 1 million * (5 bytes for word  + 15 bytes for suggestions) = 20 MB -> can be easily stored on client's device

2. For Google search bar to suggest top 10 search terms:
- There are 1 billion unique search queries per day with average length of 20 characters (around 4 words) -> to store counts per search term: 1 billion * 4 bytes(int) = 4 GB
- To store prefix for each search term ->(20*1 billion) = 20 billion entries 
- 20 billion * (10 search terms * 20 characters per term + prefix) = 20 billion * 200 bytes per search query = 4 TB -> can't be stored on client's device nor on single server

### Typeahead Suggestion Design Approach
##### **Local Non Caching Approach**: 
- Store sorted/ordered hashmap of words with counts,
- when user types a word, query hashmap to get all words with same prefix as query using binary search
- then find top k suggestions using min heap of size k
- very slow computation : Search Complexity -> O(nlogk)

##### **Local Caching Approach**:
- Store sorted map of prefix to its top k suggestions
- Search Complexity -> O(len(prefix))
- Memory footprint wrt to each term -> O(n^2) to store all prefixes

##### **Trie Approach**:
- Store Trie of words such that each node stores top k suggestions of that prefix
- Traversing the prefix via trie takes O(1) time i.e when user type next character, getting suggestion takes O(1) time so jumping from one search term to another takes constant time.
- Memory footprint wrt to each term -> O(n)
- Using dictionary compression for popular words, creating map from short type(2 bytes) reduces storage of suggested words at each node
- Trie can be stored inmemory for faster reads


