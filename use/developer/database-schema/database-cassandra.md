# Database Schema

### 1. Introduction
At UCI, we leverage Cassandra as our database solution due to its distributed nature and remarkable data access speed. Cassandra's ability to distribute data across nodes enables seamless parallel data access, enhancing both read and write performance. we maintain a comprehensive history of bot conversations using the following schema.

### 2. Table Schema

* Table Name - XMessage

|    Column Name   |     Type           |    Description                                   |
| ---------------- | ------------------- | ------------------------------------------------ |
|       id         |        UUID         | Unique identifier for the record.                |
|    userId        |        Text         | User identifier associated with the message.     |
|     fromId       |        Text         | Identifier for the sender of the message.        |
|     channel      |        Text         | Channel through which the message was sent.      |
|    provider      |        Text         | Provider of the messaging service.               |
|   timestamp      | LocalDateTime       | Date and time when the message was timestamped.  |
| messageState     |        Text         | Current state or status of the message.          |
|   xMessage       |        Text         | Content of the message.                          |
|      app         |        Text         | App is the name of bot                           |
|    auxData       |        Text         | Currently not using     |
|   messageId      |        Text         | Unique identifier for fcm callback.               |
|    replyId       |        Text         | We can store id which provied by your provider like gupshup, netcore etc. |
|    causeId       |        Text         | Same as the messageId         |
|   sessionId      |        UUID         | Random UUID               |
|   ownerOrgId     |        Text         | Identifier for the owning organization.           |
|     ownerId      |        Text         | Identifier for the owner of the message.         |
|     botUuid      |        UUID         | Unique identifier for the associated bot.        |
|     tags         | List < Text >        | List of tags or labels associated with the message. |
|   respMsgId      |        Text         | Identifier for a response message (FCM uses).                |
|    remarks       |        Text         | Additional remarks or comments about the message. |
---


