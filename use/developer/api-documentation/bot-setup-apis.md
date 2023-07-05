# Bot Setup APIs

## 1. Overview

UCI provides a list of apis that will help you to configure bots for conversation or broadcasting.&#x20;

## 2. APIs

To setup a bot and start conversation with users, the following steps need to be followed.

### 2.1 Add adapter&#x20;

Add a new adapter for the available channel & providers. Below is a list of available parameters.

* **name:** Name of the adapter
* **channel**: Messaging channel.
* **provider**: Message provider
* **config**:
  * **credentials**:
    * **vault**: Vault service name, Default value: samagra&#x20;
    * **variable**: Variable name added in the vault.

List of available channel and provider combinations:

* provider: netcore, channel: whatsapp
* provider: gupshup, channel: whatsapp
* provider: gupshup, channel: sms
* provider: cdac, channel: sms
* provider: firebase, channel: web

Note: If you need some other channel or provider, contribute a new adapter for the same. [Click here](../contribution-guide/create-an-adapter.md) to see how to do this.

Below is a curl request for adding a new adapter in db. This adapter will be associated with a bot to determine the channel and provider for the bot when conversing.

```
curl --location --request POST 'http://UCI_API:PORT/admin/adapter' \
    --header 'admin-token: {ADMIN_TOKEN}' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "data": {
            "name": "UCI Gupshup Whatsapp",
            "channel": "whatsapp",
            "provider": "gupshup",
            "config": {
                "credentials": {
                    "vault": "samagra",
                    "variable": "uci-gupshup-whatsapp"
                }
            }
        }
    }'  
```

This API will create a new adapter and will return the adapter id (Eg. 2a704e82-132e-41f2-9746-83e74550d2ea). We can use this adapter id later in [create conversation logic](#2.4-Add-a-Conversation-Logic) api.

### 2.2 Creating a Conversation Bot

A conversation bot is used to communicate with the user one on one. A chatbot uses a flow defined in an odk xml form to chat with a user. The steps for creating a simple conversation chatbot are given below.

### 2.2.1 Upload ODK Form

For using ODK transformer, we will first have to upload a ODK form. Follow below steps to upload a ODK form.

*   Convert a ODK Excel form to XML form using [Link](https://getodk.org/xlsform/).

*   Here is a [Sample ODK Excel Form](https://github.com/samagra-comms/docker-deploy/blob/main/media/List-QRB-Test-Bot.xlsx) for reference.
*   Upload this XML from using this api.

    ```
    curl --location 'http://UCI_API:PORT/admin/form/upload' \
    --header 'admin-token: {ADMIN_TOKEN}' \
    --form 'form=@"{PATH_TO_YOUR_XML}"' \
    --form 'mediaFiles=@"{PATH_TO_YOUR_MEDIA_1}"' \
    --form 'mediaFiles=@"{PATH_TO_YOUR_MEDIA_2}"' \
    --form 'mediaFiles=@"{PATH_TO_YOUR_MEDIA_N}"'
    ```

    **Response**: The API will return a form id. Use this form id to create conversation logic API. Form id E.g. **testing_form**

    ```
    {
        "apiId": "api.form.upload",
        "path": "/admin/form/upload",
        "msgid": "fec3421b-cc04-42e2-9ce9-e004b3ded827",
        "result": {
            "status": "UPLOADED",
            "data": {
                "formID": "testing_form"
            }
        },
        "startTime": "2023-07-04T12:01:00.268Z",
        "method": "POST",
        "endTime": "2023-07-04T12:01:21.785Z"
    }
    ```

### 2.2.2 Add a Conversation Logic

For any bot we will have to specify a certain configuration which is a part of conversation logic. Below is a list of available parameters.

* **name:** Conversation logic unique name
  * **transformers:** Array of transformers
    * **id:** id of transformer
    * **meta:**
      * **formID:** uploaded odk form id from [upload form api](bot-setup-apis.md#2.2-upload-odk-form) (For ODK transformer)
      * **params:** Array of parameters to be used in template **** message (For Broadcast transformer)
      * **body:** (For Broadcast transformer)
      * **type:** type of template (For Broadcast transformer), Default. **JS\_TEMPLATE\_LITERALS** &#x20;
    * **type:** type of transformers (Eg. broadcast/generic) ****&#x20;
  * **adapter:** id **** of adapter from [add adapter api](bot-setup-apis.md#2.1-add-adapter). &#x20;

Use below curl to create a conversation logic.

```
curl --location 'http://UCI_API:PORT/admin/conversationLogic' \
--header 'asset: conversationLogic' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json, text/plain, */*' \
--header 'ownerOrgID: org01' \
--header 'ownerID: 8f7ee860-0163-4229-9d2a-01cef53145ba' \
--data '{
    "data": {
        "name": "TEST",
        "description": "TEST",
        "transformers": [
            {
                "id": "774cd134-6657-4688-85f6-6338e2323dde",
                "meta": {
                    "form": "https://hosted.my.form.here.com",
                    "formID": "testing_form",
                    "title": "TEST",
                    "body": "TEST",
                    "serviceClass": "SurveyService",
                    "hiddenFields": [
                        {
                            "name": "mobilePhone",
                            "path": "mobilePhone",
                            "type": "param",
                            "config": {
                                "dataObjName": "user"
                            }
                        }
                    ],
                    "templateType": "JS_TEMPLATE_LITERALS",
                    "type": "broadcast"
                }
            }
        ],
        "adapter": "6efa8087-0939-49ab-b8e5-5676e036c17b"
    }
}'
```

**Response**: It will return a conversation logic id, use it in create bot. Eg. **be37c9f2-7f2b-4e19-b525-33b0a2aabdd5**

```
{
    "apiId": "api.admin.conversationLogic",
    "path": "/admin/conversationLogic",
    "msgid": "df8edb08-f887-41cd-8f7a-604d3229dabc",
    "result": {
        "id": "be37c9f2-7f2b-4e19-b525-33b0a2aabdd5",
        "name": "TEST",
        "createdAt": "2023-07-05T05:29:57.310Z",
        "updatedAt": "2023-07-05T05:29:57.311Z",
        "description": null,
        "adapterId": "6efa8087-0939-49ab-b8e5-5676e036c17b"
    },
    "startTime": "2023-07-05T05:29:57.293Z",
    "method": "POST",
    "ownerId": "8f7ee860-0163-4229-9d2a-01cef53145ba",
    "endTime": "2023-07-05T05:29:57.314Z"
}
```

### 2.2.3 Configure a bot
After the conversation logic is defined, we can use this to create a new bot. This bot must include a starting message as we will use this to start a conversation for this bot. Below is a list of available parameters.


* **startingMessage:** unique message to start conversation.
* **name:** unique name of bot.
* **users:** array of user segment ids, currently the first one will be used.
* **logic:** array of conversation logic ids from add logic api, currently the first one will be used.
* **status:** status of bot, Eg. draft/disabled/enabled.
* **startDate:** date from which bot will be active.
* **endDate:** date till which bot will be active.

Use below curl to create a conversation logic.

```
curl --location --request POST 'http://UCI_API:PORT/admin/bot' \
--header 'admin-token: {ADMIN_TOKEN}' \
--form 'botImage=@"/path/to/file"' \
--form 'data="{
    \"data\": {
        \"startingMessage\": \"Hi Bot\",
        \"name\": "UCI Demo Bot\",
        \"users\": [],
        \"logic\": [\"be37c9f2-7f2b-4e19-b525-33b0a2aabdd5\"],
        \"status\": \"enabled\",
        \"startDate\": \"2023-05-4\",
        \"endDate\": \"2025-12-01\"
    }
}"'
```


Response: This api will return a bot id & other bot information. Use the starting message (Eg. Hi Bot) from here to start a conversation with a bot.

### 2.3 Creating a Broadcast Bot (Optional)
### 2.3.1 Add a User Segment

User segment contains the group of users to whom the broadcast would be sent out to.

Use the curl below to create a user segment.

```
curl --location 'http://UCI_API:PORT/admin/user-segment' \
--header 'asset: userSegment' \
--header 'admin-token: {ADMIN_TOKEN}' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json, text/plain, */*' \
--header 'ownerOrgID: org01' \
--header 'ownerID: 8f7ee860-0163-4229-9d2a-01cef53145ba' \
--data '{
    "name": "TEST_USER_SEGMENT",
    "all": {
        "type": "get",
        "config": {
            "url": "http://testSegmentUrl/segments/1/mentors?deepLink=nipunlakshya",
            "type": "GET",
            "cadence": {
                "perPage": 100,
                "retries": 5,
                "timeout": 60,
                "concurrent": true,
                "pagination": true,
                "concurrency": 10,
                "retries-interval": 10
            },
            "pageParam": "page",
            "credentials": {}
        }
    },
    "byID": {
        "type": "get",
        "config": {
            "url": "http://testSegmentUrl/segments/1/mentors?deepLink=nipunlakshya://chatbot",
            "type": "GET",
            "cadence": {
                "perPage": 100,
                "retries": 5,
                "timeout": 60,
                "concurrent": true,
                "pagination": true,
                "concurrency": 10,
                "retries-interval": 10
            },
            "pageParam": "page",
            "credentials": {}
        }
    },
    "byPhone": {
        "type": "get",
        "config": {
            "url": "http://testSegmentUrl/segments/1/mentors?deepLink=nipunlakshya://chatbot",
            "type": "GET",
            "cadence": {
                "perPage": 100,
                "retries": 5,
                "timeout": 60,
                "concurrent": true,
                "pagination": true,
                "concurrency": 10,
                "retries-interval": 10
            },
            "pageParam": "page",
            "credentials": {}
        }
    }
}'
```

**Response**: It will return a response containing the id of the segment that is created. Eg. **2ddea169-2db9-442b-99f2-fc85c9ea1a02**

```
{
    "id": "2ddea169-2db9-442b-99f2-fc85c9ea1a02",
    "createdAt": "2023-07-05T06:02:16.285Z",
    "updatedAt": "2023-07-05T06:02:16.285Z",
    "name": "TEST_JUL_5",
    "description": null,
    "count": 0,
    "category": null,
    "allServiceID": "00b775e2-fae9-439f-8e9d-3231dc5aed46",
    "byPhoneServiceID": "a279516d-3730-4b7f-b74c-de5df0206177",
    "byIDServiceID": null,
    "botId": null
}
```



### 2.2.4 Add a Broadcast bot

After the conversation logic is defined, we can use this to create a new bot. This bot must include a starting message as we will use this to start a conversation for this bot. Below is a list of available parameters.

* **startingMessage:** unique message to start conversation
* **name:** unique name of bot ****&#x20;
* **users:** array of user segment ids, currently the first one will be used
* **logic:** array of conversation logic ids from [add logic api](bot-setup-apis.md#2.3-create-a-conversation-logic), currently the first one will be used&#x20;
* **status:** status of bot, Eg. draft/enabled
* **startDate:** date from which bot will be active
* **endDate:** date till which bot will be active

Use below curl to create a conversation logic.

```
curl --location 'http://http//UCI_API:PORT/admin/bot' \
--header 'asset: bot' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
--header 'Accept: application/json, text/plain, */*' \
--header 'ownerOrgID: org01' \
--header 'ownerID: 8f7ee860-0163-4229-9d2a-01cef53145ba' \
--form 'botImage=@"{PATH_TO_IMAGE}"' \
--form 'data="{
    \"data\": {
    \"name\": \"TEST BROADCAST BOT\",
    \"description\": \"TEST\",
    \"purpose\": \"TEST\",
    \"startingMessage\": \"Hi Test Broadcast\",
    \"startDate\": \"2023-06-16\",
    \"endDate\": \"2023-06-30\",
    \"isBroadcastBotEnabled\": true,
    \"segmentId\": \"1\",
    \"status\": \"enabled\",
    \"users\": [
        \"2ddea169-2db9-442b-99f2-fc85c9ea1a02\" // Get this id from user segment api response
    ],
    \"logic\": [
      \"be37c9f2-7f2b-4e19-b525-33b0a2aabdd5\" // Get this id from conversation logic api response
    ]
  }
}"'
```

**Response**: This api will return a bot id & other bot information. Use the starting message (Eg. **Hi Test ODK**) from here to start conversation with a bot.

```
{
    "apiId": "api.admin.bot",
    "path": "/admin/bot",
    "apiVersion": "v1",
    "msgid": "ba858dbb-8710-40e3-9fca-e8f928ff2cfc",
    "result": {
        "id": "5066c217-6ae0-4e3c-9802-8c0cce660c2b",
        "createdAt": "2023-07-04T12:59:48.804Z",
        "updatedAt": "2023-07-04T12:59:48.804Z",
        "name": "TEST ODK BROADCAST",
        "startingMessage": "Hi Test Broadcast",
        "ownerID": null,
        "ownerOrgID": null,
        "purpose": null,
        "description": null,
        "startDate": "2023-06-15T00:00:00.000Z",
        "endDate": "2025-12-01T00:00:00.000Z",
        "status": "ENABLED",
        "tags": [],
        "botImage": "1c8516c6-c4c3-4a37-a8dd-89109e8e0e60.png"
    },
    "startTime": "2023-07-04T12:59:48.265Z",
    "method": "POST",
    "endTime": "2023-07-04T12:59:49.097Z"
}
```
