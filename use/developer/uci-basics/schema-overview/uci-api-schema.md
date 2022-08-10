# UCI API Schema

UCI API schema is keeps the information regarding the bot and its configuration. When a new bot is created via [APIs](../../api-documentation/bot-setup-apis.md) its data will be stored in this Database.&#x20;

## Schema

### Adapter

This table keeps the adapter information. These adapter will be associated to the [conversation logic](../../api-documentation/bot-setup-apis.md#2.3-add-a-conversation-logic) to determine the adapter to be used while having the conversation. API to add a new adapter is given [here](../../api-documentation/bot-setup-apis.md#2.1-add-adapter).&#x20;

| Field       | Type      | Description                                         |
| ----------- | --------- | --------------------------------------------------- |
| id          | uuid      |                                                     |
| channel     | text      | Eg. whatsapp/sms                                    |
| provider    | text      | Eg. netcore/gupshup                                 |
| config      | jsonb     | json configuration for adapter, Eg. its credentials |
| name        | text      | adapter name                                        |
| created\_at | timestamp |                                                     |
| updated\_at | timestamp |                                                     |

### Transformer

This table store the transformers info and configuration, which are already implemented and working in UCI. [Click here](../transformers/) to know what are transformers & how they work.

| Field       | Type      | Description                  |
| ----------- | --------- | ---------------------------- |
| id          | uuid      |                              |
| name        | text      |                              |
| tags        | text\[]   |                              |
| config      | jsonb     | json configuration           |
| service\_id | uuid      | foreign key of service table |
| created\_at | timestamp |                              |
| updated\_at | timestamp |                              |

### Service

This table store the info to use when fetching the users data. The service will be added with user segment, which later will be added in the bot table when creating [one](../../api-documentation/bot-setup-apis.md#2.4-add-a-bot).

| Field       | Type      | Description                                                                    |
| ----------- | --------- | ------------------------------------------------------------------------------ |
| id          | uuid      |                                                                                |
| type        | text      | type of service Eg. gql/get                                                    |
| config      | jsonb     | json configuration for fetching data from, Eg. get url/gql query configuration |
| name        | text      |                                                                                |
| created\_at | timestamp |                                                                                |
| updated\_at | timestamp |                                                                                |

### User Segment

This table store the details to fetch the users data for different conditions like all, by id or by phone number.&#x20;

| Field       | Type      | Description                                                                          |
| ----------- | --------- | ------------------------------------------------------------------------------------ |
| id          | uuid      |                                                                                      |
| name        | text      |                                                                                      |
| all         | uuid      | service table id to be used when fetching the all users data                         |
| byId        | uuid      | service table id to be used when fetching the users data for a specific id           |
| byPhone     | uuid      | service table id to be used when fetching the users data for a specific phone number |
| created\_at | timestamp |                                                                                      |
| updated\_at | timestamp |                                                                                      |

### Conversation Logic

This table stores the bot related logic like form id or message body. [Click here](../../api-documentation/bot-setup-apis.md#2.3-add-a-conversation-logic) to see the API to add conversation logic.

| Field        | Type      | Description                       |
| ------------ | --------- | --------------------------------- |
| id           | uuid      |                                   |
| name         | text      |                                   |
| transformers | jsondb    | json array of transformers config |
| adapter      | uuid      | id of adapter table               |
| description  | text      |                                   |
| created\_at  | timestamp |                                   |
| updated\_at  | timestamp |                                   |

### Bot

This table keeps the details of bot name, starting message, [logic](uci-api-schema.md#conversation-logic) to use, [user segment](uci-api-schema.md#user-segment) to use etc. API to add a bot can be found [here](../../api-documentation/bot-setup-apis.md#2.4-add-a-bot).

| Field            | Type    | Description                                    |
| ---------------- | ------- | ---------------------------------------------- |
| id               | uuid    |                                                |
| name             | text    | bot name for identification                    |
| starting message | text    | a message to start a conversation on a channel |
| users            | text\[] | array of user segment ids                      |
| logicIDs         | text\[] | array of conversation logic ids                |
| status           | text    | Eg. enabled/draft                              |
| start\_date      | date    | conversation activation date                   |
| end\_date        | date    | conversation de-activation date                |
| ownerId          | text    | id of owner of this bot                        |
| ownerOrgId       | text    | org id of owner of this bot                    |
| purpose          | text    | purpose of the bot creation                    |

### &#x20;
