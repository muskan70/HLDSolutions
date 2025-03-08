## Chat System Design

### Functional Requirements
1. System should support chat between 2 people as well as group chat.
2. Group can consist of maximum 100 people.
3. User should be able to see presence status i.e online, last seen of friends.
4. User can see history of chats in cronological order.
5. User should be able to send and receive messages in real time.
6. If user is offline, he should be notified about incoming messages.

### Non Functional Requirements
1. Scalabilty: lets assume 1 billion users with each user sending 100 messages daily.
2. Availability and Fault tolerant.

### Back of Envelope Estimation
1.

### API Design
1. For communicating we can use web sockets as it is bidirectional communication.
2. For presence

### Database Design
1. FollowerDB 
- Schema ->{UserId, FollowerID}
- Graph DB

2. OneToOneChatDB
- Schema -> {messageID, content, messageFrom, messageTo, timestamp}
- Relational DB, Sharded on ChatId, sorted on timestamp

2. GroupChatDB
- Schema -> {messageID, content, groupID, userID, timestamp}
- Relational DB, Sharded on ChatId, sorted on timestamp

### System GoThrough