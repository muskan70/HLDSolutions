## Chat System Design

### Functional Requirements
1. System should support chat between 2 people as well as group chat.
2. User should be able to create Group and it can consist of maximum 100 people.
3. User should be able to see presence status i.e online, last seen of friends.
4. User can see history of chats in cronological order.
5. User should be able to send and receive messages, media in real time.
6. If user is offline, he should be notified about incoming messages.

### Non Functional Requirements
1. Scalabilty: lets assume 1 billion users with each user sending 100 messages daily.
2. Consistent and Fault tolerant.
3. Users should be able to send and receive messages with minimum latency.

### Back of Envelope Estimation
1. Average message size = 100 bytes(including metadata)
2. Storage = 1 billion * 100 * 100 bytes = 10 TB per day ~ 4 PB per year

### Communication Between Client and servers Design
**Stateful Services**
1. For send/receive messages between client and chat servers, instead of using http requests or polling or long pooling we can use web sockets as it is bidirectional communication.
2. Similarly for presence updates, use of heartbeat mechanism will be better where client send heartbeat events event 5s to presence servers, if server doesn't receive hearbeat within next 30s client will be considered offline.

**Stateless Services**
1. POST /api/v1/user/new
- Request Body Parametres => name, email, phone, password
- Returns userID

2. POST /api/v1/createGroup
- Request Body Parametres => Group Name, Admin UserId, List{Users}
- Returns chatID

### Database Design
1. UserDB
- Schema -> {UserID, name, email, phone, password, imageURL, aboutContent}
- MySQL, Sharded on userId

2. Chat Member DB 
- Schema -> {ChatID, UserId}
- MySQL, Sharded on UserID -> to get all chatIDs related to user

3. MessageDB
- Schema -> {ChatId, messageID, content, messageFrom, timestamp}
- Key Value DB or Hbase, Partitioned on ChatId, sorted on timestamp
- Also assign UUID for idempotency of message pushing 

### System GoThrough


