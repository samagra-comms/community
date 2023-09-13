# Database Schema

### Introduction
This database is built on PostgreSQL and manages all ODK questions and answers, as well as their respective states. It tracks the questions posed to users and the anticipated follow-up inquiries.

### 1. Table Schema

* Table Name - question


|    Column Name   |     Type           |    Description                                   |
| ---------------- | ------------------- | ------------------------------------------------|
|       id         |        UUID         | Unique identifier for the record.                |
|     form_id       |      Varchar       | This is odk formid, while we create odk form we added in the xml. |
|   form_version    |      Varchar       | This is odk form version, while we create odk form we added in the xml.  |
|      x_path       |      Varchar       | This is path of question get from odk form.      |
|  question_type    |      Varchar       | Type of the question like String, Single Select and Multi-Select. |
|      meta        |       Jsonb        | JSON data structure its contain the question which we sent to our users. |
|   updated      |     Timestamp      | Timestamp indicating when the record was last updated. |
|   created      |     Timestamp      | Timestamp indicating when the record was created. |
---

### 2. Table Schema

* Table Name - xmessage

|  Column Name     |      Type          |        Description                                     |
| ---------------- | ------------------- | ------------------------------------------------------- |
|       id         |       Long          | Unique identifier for the record.                       |
|    phone_no       |      Varchar        | Phone number associated with the user.               |
|    message       |      Varchar        | The content of the message which we sent to user.                             |
| is_last_message   |      Boolean        | A boolean flag indicating if this is the last response in a series of messages.|
---


### 3. Table Schema

* Table Name - xmessage_state

|  Column Name     |      Type          |        Description                                     |
| ---------------- | ------------------- | ------------------------------------------------------- |
|       id         |       Long          | Unique identifier for the record.                       |
|    phoneNo       |      Varchar        | Phone number associated with the user.                |
|     state        |        Text         | XML data for previous question.              |
|  previous_path   |      Varchar        | Path or location of the previous question.              |
|  bot_form_name   |      Varchar        | Name or identifier of the bot form associated with the bot.|
---
