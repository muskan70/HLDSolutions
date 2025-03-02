## Notification Service System Design

### Functional Requiremnts
1. Service should accept notification requests from various types of servers.
2. It should support different types of notifications like email, SMS, Whatsapp, push notifications.

### Non Functional Requiremts
1. Scalability: System should be able to handle 1 million notification per day
2. Idempotence: System should be able to send notifications exactly once to the client.
3. Fault Tolerant

### API Design
1. POST /api/v1/send?{notification-type}
- Request Body:{messsage: xxxx, userId:xxxx}

### Database Design
1. **User Notification Settings Table**:  Relational DB, Single Leader Replication, Partitioned by UserId
> - UserId: int
> - Notification Type: { email/sms/whatapp/push}
> - OptIn :bool

2. **Notification Log Table**: Cassandra, Multiple Leader Replication, Partioned By TopicId and Indexed By Timestamp
> - NotificationId: int
> - Status:{pending, failed, completed}
> - Retries left: int
> - Last Sent At: timestamp
> - Message: string
> - Notification Type: { email/sms/whatapp/push}
> - TopicId: string

3. **Template Table**: Key Value
> - TemplateId : int 
> - template: string
> - createdAt: timestamp

### System Go Through