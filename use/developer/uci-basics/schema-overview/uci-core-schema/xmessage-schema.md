# XMessage Schema

UCI saves all conversation data in form of XMessages. These XMessages follow a certain [specification](../../xmessage-specification.md). These XMessage will be saved in cassandra DB. Its object is defined in [dao repository](../../#2.3.-dao).

## Schema

| Field        | Type      | Description                    |
| ------------ | --------- | ------------------------------ |
| id           | uuid      | unique identification          |
| userId       | text      | to - phone of user/admin       |
| fromId       | text      | from - phone of user/admin     |
| channel      | text      | channel (Eg. Whatsapp/SMS)     |
| provider     | text      | provider (Eg. Gupshup/Netcore) |
| timestamp    | timestamp |                                |
| messageState | text      | state(Eg. Replied/Sent/Read)   |
| xMessage     | text      | xMessage XML stringyfied       |
| app          | text      | bot name                       |
| auxData      | text      | auxilary data                  |
| messageId    | text      | id of message from provider    |
| replyId      | text      | id of user to reply to         |
| causeId      | text      |                                |
| sessionId    | uuid      | session id of conversation     |
| ownerOrgId   | text      | owner org id of bot            |
| ownerId      | text      | owner id                       |
| botUuid      | uuid      | bot uuid                       |
