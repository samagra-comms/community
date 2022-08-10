# Assessment DB Schema

UCI uses postgreSQL for storing data of conversations. ODK transformer uses this database to store its assessment data for each question & answer.

## Schema

Assessment data consists of 4 tables, which are listed below.

### Question

This table keeps the details of question asked to user.&#x20;

| Field          | Type         | Description                     |
| -------------- | ------------ | ------------------------------- |
| id             | uuid         |                                 |
| form\_id       | varchar(100) | odk form id                     |
| form\_version  | varchar(10)  | odk form version                |
| x\_path        | varchar(500) | odk question xpath              |
| question\_type | varchar(500) |                                 |
| meta           | jsonb        | extra meta data in form of json |
| created        | timestamp    |                                 |
| updated        | timestamp    |                                 |

### Assessment

This table keeps the details of answer submitted by user for a specific question.

| Field      | Type      | Description                       |
| ---------- | --------- | --------------------------------- |
| id         | uuid      |                                   |
| question   | uuid      | id of table question              |
| answer     | text      | answer given by user for question |
| bot\_id    | uuid      | bot id                            |
| user\_id   | uuid      | fusion auth returned user id      |
| device\_id | uuid      | fusion auth returned user id      |
| meta       | jsondb    | extra meta data in form of json   |
| created    | timestamp |                                   |
| updated    | timestamp |                                   |

### Xmessage

This table keeps the records of messages sent to user against his/her phone number.

| Field             | Type          | Description          |
| ----------------- | ------------- | -------------------- |
| id                | bigint        |                      |
| phone\_no         | varchar(15)   | phone number of user |
| updated\_at       | timestamp     |                      |
| message           | varchar(1000) | message sent to user |
| is\_last\_message | boolean       |                      |

### XMessage State

This table keeps the odk instance xml state against the user phone number and form name.&#x20;

| Field           | Type          | Description               |
| --------------- | ------------- | ------------------------- |
| id              | bigint        |                           |
| phone\_no       |  varchar(15)  | phone number of user      |
| state           | text          | current xml state of form |
| previous\_path  |  varchar(100) | previous question xpath   |
| bot\_form\_name | varchar(50)   | odk form name             |
| updated\_at     | timestamp     |                           |
