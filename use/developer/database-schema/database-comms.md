# Database Schema

### Introduction
In this database, we utilize PostgreSQL to store all the pertinent information related to our chatbot. This includes details such as the adapter we are using, the type of transformer employed, and the number of users to whom we will send notifications or SMS messages. In essence, this database exclusively contains information about the chatbot.

### 1. Table Schema

* Table Name - adapter

|  Column Name   |      Type     |     Description                            |
| -------------- | ------------- | ------------------------------------------ |
|      id        |      UUID     | Unique identifier for the record.           |
|   createdAt    |  Timestamp    | Timestamp indicating when the record was created. |
|   updatedAt    |  Timestamp    | Timestamp indicating when the record was last updated. |
|    channel     |     Text      | This field identifies the communication channel utilized, such as SMS, WhatsApp, or web-based platforms, among others. |
|   provider     |     Text      | Name of the service provider, such as Netcore, Gupshup, Firebase, etc., responsible for delivering messages or services. |
|    config      |     Jsonb     | JSON data structure storing configuration information related to service providers. This field may include vault key credentials and other provider-specific settings. |
|     name       |     Text      | This field store name of adapter |
---


### 2. Table Schema

* Table Name - Service

|   Column Name       |       Type       |       Description                                           |
| ------------------- | ----------------- | ------------------------------------------------------------ |
|         id          |       UUID       | Unique identifier for the service record.                    |
|      createdAt      |    Timestamp     | Timestamp indicating when the service record was created.     |
|      updatedAt      |    Timestamp     | Timestamp indicating when the service record was last updated.|
|         type        |       Text       | Type of the service like odk, gql, get. use for user segment. |
|        config       |      JsonB       | JSON data structure storing configuration information related to the service like user segment url, cadence, total records etc. |
|         name        |       Text       | Name or title of the service.                                 |
---

### 3. Table Schema

* Table Name - Transformer

|   Column Name   |     Type     |            Description                                      |
| --------------- | -------------| ------------------------------------------------------------- |
|       id        |     UUID     | Unique identifier for the transformer record.                |
|    createdAt    |  Timestamp   | Timestamp indicating when the transformer record was created. |
|    updatedAt    |  Timestamp   | Timestamp indicating when the transformer record was last updated. |
|      name       |     Text     | Name or title associated with the transformer.               |
|      tags       |     Text     | Tags or labels associated with the transformer like Odk, Generic, Broadcast.   |
|     config      |    Jsonb     | JSON data structure storing configuration information related to the transformer. |
|   serviceId     |     UUID     | Foreign key referencing the service to which the transformer is related. |
---



### 4. Table Schema

* Table Name - TransformerConfig

|   Column Name         |     Type     |            Description                                      |
| --------------------- | -------------| ------------------------------------------------------------- |
|       id              |     UUID     | Unique identifier for the transformer configuration record.   |
|    createdAt          |  Timestamp   | Timestamp indicating when the configuration record was created. |
|    updatedAt          |  Timestamp   | Timestamp indicating when the configuration record was last updated. |
|  transformerId        |     UUID     | Foreign key referencing the associated transformer.          |
|      meta             |    Jsonb     | JSON data structure storing additional metadata related to the configuration like title, body, hiddenFields, formID, form, serviceClass, templateType etc. |
| conversationLogic     |     UUID     | Foreign key referencing the conversation logic associated with the configuration. |
---

### 5. Table Schema

* Table Name - ConversationLogic

|   Column Name   |     Type     |       Description                                       |
| --------------- | -------------| -------------------------------------------------------- |
|       id        |     UUID     | Unique identifier for the conversation logic record.    |
|    createdAt    |  Timestamp   | Timestamp indicating when the conversation logic record was created. |
|    updatedAt    |  Timestamp   | Timestamp indicating when the conversation logic record was last updated. |
|   description   |     Text     | Description providing details about the conversation logic. |
|    adapterid    |     UUID     | Foreign key referencing the associated adapter.          |
|      name       |     Text     | Name or title associated with the conversation logic.    |
---


### 6. Table Schema

* Table Name - bot

|   Column Name       |       Type       |       Description                                           |
| ------------------- | ----------------- | ------------------------------------------------------------ |
|         id          |       UUID       | Unique identifier for the bot record.                             |
|      createdAt      |    Timestamp     | Timestamp indicating when the bot record was created.            |
|      updatedAt      |    Timestamp     | Timestamp indicating when the bot record was last updated.       |
|         name        |       Text       | The name of the bot.                  |
|  startingMessage    |       Text       | The initial message or greeting used by the bot when interacting with users.              |
|      ownerID        |       Text       | Identifier of the owner or creator of the bot.          |
|     ownerOrgID      |       Text       | Identifier of the organization to which the bot owner belongs. |
|      purpose        |       Text       | A brief description of the purpose or function of the bot.        |
|    description      |       Text       | A detailed description of the bot, including its capabilities and use cases.          |
|     startDate       |       Date       | The date when the bot became active or started operating.                         |
|      endDate        |       Date       | The date when the bot's operation is scheduled to end or expire.                            |
|       status        |    BotStatus     | The current status of the bot, indicating whether it is enabled or disabled. (ENABLED, DISABLED) |
|        tags         |       Text       | Tags or labels associated with the bot for categorization or organization.                    |
|     botImage        |       Text       | Bot image icon.     |
---

### 7. Table Schema

* Table Name - UserSegment

|   Column Name    |     Type     |       Description                                       |
| ---------------- | -------------| -------------------------------------------------------- |
|        id        |     UUID     | Unique identifier for the user segment record.          |
|     createdAt    |  Timestamp   | Timestamp indicating when the user segment record was created. |
|     updatedAt    |  Timestamp   | Timestamp indicating when the user segment record was last updated. |
|       name       |     Text     | Name or title associated with the user segment.          |
|    description   |     Text     | Description providing details about the user segment.    |
|      count       |     Int      | Count or number of users in the segment.                |
|     category     |     Text     | Category or type of the user segment.                   |
|  allServiceID    |     UUID     | Unique identifier referencing the associated service for all users in the segment. |
| byPhoneServiceID |     UUID     | Unique identifier referencing the associated service for users in the segment by phone. |
|    byIDServiceID |     UUID     | Unique identifier referencing the associated service for users in the segment by ID. |
|       botId      |     UUID     | Unique identifier referencing the associated bot for the user segment. |
---