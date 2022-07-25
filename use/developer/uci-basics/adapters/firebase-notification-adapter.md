# Firebase Notification Adapter

## Firebase Adapter

### 1. Overview

UCI provides a firebase adapter which sends push notifications to your Mobile or Web based application.

### 2. How to send firebase notification ?

We need a mobile or web application that will register the FCM tokens on firebase. These tokens can be later used to trigger firebase notification. We can trigger these notification in two ways.

#### 1. Use UCI Internal Apis

We can store these registered tokens in UCI & use UCI fetch api to get these tokens for notification. Follow below steps to do this.

1.  Register FCM tokens on any mobile or web application using the [firebase details of UCI](firebase-notification-adapter.md#3.-contact-the-administrator). Add this FCM token on UCI using below API:

    ```
    curl --location --request POST 'http://143.110.255.220:8085/fusionAuth/registerUserFcmToken' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "device": {
            "id":"cytMfcuBgpembxn7GfNvDh:APA91bGhiETdrh7Kq5ERG1XTQ6k53p9pVZOnMOmMFIVJVy_h4rwFb03F9AWsJ9Y51-Pc_DCt1kppHhw976WjTA_mh6WZO1pBll2A2DrUNFvG6uOjpQhc1WQoFtQUvxc3ka9e_TbBriu8",
            "type":"fcm"
        },
        "users": [
            {
                "registrationChannel": "uci-web-channel",
                "username": "john",
                "mobilePhone": "7597185712"
            }
        ]
    }'
    ```
2. The tokens registered can be fetched via the [API](http://143.110.255.220:8080/fusionAuth/fetchFcmTokens).
3. Create a new service [here](http://143.110.255.220:15003/console/data/default/schema/public/tables/service/browse) with the fetch token API in config.
4. Create a new user segment [here](http://143.110.255.220:15003/console/data/default/schema/public/tables/userSegment/browse) with the service created in step 3. Or you can use an already created user segment for this case. `cf085492-266f-46d1-841c-1b38e93bce3f`.
5.  Create a new conversation logic with below logic

    ```
    curl --location --request POST 'http://143.110.255.220:9999/admin/v1/conversationLogic/create' \
    --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "data": {
            "name": "Firebase Broadcast Logic",
            "transformers": [
                {
                    "id": "774cd134-6657-4688-85f6-6338e2323dde",
                    "meta": {
                        "body": "Hello ${name}-${phoneNo}, Test Notification",
                        "type": "JS_TEMPLATE_LITERALS",
                        "params": [
                            "name",
                            "phoneNo"
                        ]
                    },
                    "type": "broadcast"
                }
            ],
            "adapter": "2a704e82-132e-41f2-9746-83e74550d2ea"
        }
    }'
    ```
6.  Create a new bot with the conversation logic from step 5, user segment from step 4.

    ```
    curl --location --request POST 'http://143.110.255.220:9999/admin/v1/bot/create' \
    --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "data": {
            "startingMessage": "Test Firebase Notification",
            "name": "Test Firebase Notification Bot",
            "users": ["cf085492-266f-46d1-841c-1b38e93bce3f"],
            "logic": [
                "03ec1ac2-76b3-496a-8184-9e9e9d01c048"
            ],
            "status": "enabled",
            "startDate": "2022-06-13",
            "endDate": "2023-06-13"
        }
    }'
    ```
7.  Hit the below api using the created bot id to send firebase notification to all registered FCM tokens.

    ```
    curl --location --request GET 'http://143.110.255.220:9999/campaign/start?campaignId=1ea5346d-8d98-4cf4-a470-9c234476f3d1'
    ```

#### 2. Use your own FCM token api

We can also use third party apis that have registered FCM tokens in a specific format. For this we will also need the firebase project service key to send notifications.

1.  Create an api that provides the FCM tokens in below format.

    ```
        {
            "data": [
                {
                    "fcmToken": "cytMfcuBgpembxn7GfNvDh:APA91bHsaJioCNAoWAZql1lxe4szwd74CJsPEVp1ngSCrzMcft6kB9ZrZnUZ9PUVU47kGiVSUAk70ayF55nzi8vU6jlWI4AGLkTo9ZBZnwMll0ZqVKioAQARvgE4GTUwIoTWAqVUYGMN",
                    "phoneNo": "7597185712",
                    "name": "sim"
                },
                {
                    "fcmToken": "cytMfcuBgpembxn7GfNvDh:APA91bHsaJioCNAoWAZql1lxe4szwd74CJsPEVp1ngSCrzMcft6kB9ZrZnUZ9PUVU47kGiVSUAk70ayF55nzi8vU6jlWI4AGLkTo9ZBZnwMll0ZqVKioAQARvgE4GTUwIoTWAqVUASDF",
                    "phoneNo": "7597185713",
                    "name": "john"
                }
            ]
        }
    ```
2. Add Firebase service key in vault using below APIs.
   1.  Login API\


       ```
           curl --location --request POST 'http://auth.samagra.io:9011/api/login' \
           --header 'Authorization: YFpyHxhW0-NoKRwQrXgCU5QIAQq8nBNhE--i5_n3pTU' \
           --header 'Content-Type: application/json' \
           --header 'Cookie: refresh_token=I2RhB1AE8Ve0gpCDh4DF3ddGBvGkwuzmokIHwi-VGbZFKdo_XSTV7A' \
           --data-raw '{
               "loginId": "loginId",
               "password": "password",
               "applicationId": "a1313380-069d-4f4f-8dcb-0d0e717f6a6b"
           }'
       ```

       \
       Please [contact the administrator](../../../contact-the-administrator.md) to get the **loginId** and **password** for this API.\
       \
       This API will give below response. Use the token from the response in "Add secret API".\


       ```
           {
               "refreshToken": "-oYek_AgrkNGAP2cDTwgUkAsV72FDA-bRtbzmUS9Y-DOligCTx8FvA",
               "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjZoMng4Ty1IMl9XWWt5dG41ZTNuMzlwUEZlWSJ9.eyJhdWQiOiJhMTMxMzM4MC0wNjlkLTRmNGYtOGRjYi0wZDBlNzE3ZjZhNmIiLCJleHAiOjE2NTU4MTcyMDYsImlhdCI6MTY1NTgxMzYwNiwiaXNzIjoiYWNtZS5jb20iLCJzdWIiOiI4ZjdlZTg2MC0wMTYzLTQyMjktOWQyYS0wMWNlZjUzMTQ1YmEiLCJqdGkiOiI0OGY3ZjQ3ZS1lMGM4LTQ4NmItYjVlNC00NDNlZTVkMjc1NGEiLCJhdXRoZW50aWNhdGlvblR5cGUiOiJQQVNTV09SRCIsInByZWZlcnJlZF91c2VybmFtZSI6InVjaS11c2VyIiwiYXBwbGljYXRpb25JZCI6ImExMzEzMzgwLTA2OWQtNGY0Zi04ZGNiLTBkMGU3MTdmNmE2YiIsInJvbGVzIjpbIm9yZ093bmVyIl0sIm93bmVyT3JnSWQiOiJ1Y2ktdXNlciIsIm93bmVySWQiOiJ1Y2ktdXNlciJ9.GOwVGcbjVSXZpswoVtkwdf8Kv48PuNbquFPdkQgmfabUOyMwzo_n83JAq82r2x5S8lVg07ZAovBEB1D2Ak2Vfmhr_N0gxL3bdq8YrT_e3P37mVY-OPGHbSdwzIoYEdM9njyttGjue9UI_I_i4oUI7ENVWv9GFzJmwEaVjhaZiE0NHcV5gmic62cCIQo8N-lzd4lMdZEQnDS_AxYCvjhsw2P4MxXNqzkK8bhqEcMWosfLaBrO_WJ4eh_Py0HuTMeeTI0uzSEyKPUSoQJZHuTTfks_VnayqwnpSKYPSx_riY_pHjTuB8S1L52P3fFRlwQPxPcKgyIu26DVDptqNmqAXw",
               "user": {
                   "active": true,
                   "registrations": [
                       {
                           "applicationId": "a1313380-069d-4f4f-8dcb-0d0e717f6a6b",
                           "id": "76179ff7-cd65-4c6f-9a95-d01720eacc75",
                           "roles": [
                               "orgOwner"
                           ],
                           "username": "uci-user",
                           "usernameStatus": "ACTIVE",
                           "verified": true
                       }
                   ],
                   "username": "loginId",
                   "usernameStatus": "ACTIVE",
                   "verified": true
               }
           }
       ```

       \

   2.  Add a secret API\


       ````
           curl --location -g --request POST 'http://143.110.183.73:3001/admin/secret' \
           --header 'ownerId: 8f7ee860-0163-4229-9d2a-01cef53145ba' \
           --header 'ownerOrgId: org1' \
           --header 'Authorization: Bearer loginToken' \
           --header 'asset: secret' \
           --header 'Content-Type: application/json' \
           --data-raw '{
               "secretBody": {
                   "serviceKey": "BBHJGiL4qhQ:APA91bHfQkDIXbBbChCA3AUo5Wx9eRrAE2RWjtkLBCMxOJGmQGrUXqezKBy54xGJDegR6dM8H39r6XkSVDUOQUZ0QO9-Q_vexAM9UDCvCzZnerh8k1dIFUdIaQKdP8cRCW5KJG3TB123"
               },
               "type": "Headers",
               "variableName": "uci-firebase-notification"
           }'
           ```
       ````

       \
       This API will add the service key in vault. We will later use this secret variable name in adapter API to fetch its key for notification trigger.&#x20;
3.  Create a new adapter with credentials variable(Eg. uci-firebase-notification) added in step 2. \


    ```
        curl --location --request POST 'http://143.110.255.220:9999/admin/v1/adapter/create' \
        --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "data": {
                "name": "UCI Firebase Web Notification",
                "channel": "web",
                "provider": "firebase",
                "config": {
                    "credentials": {
                        "vault": "samagra",
                        "variable": "uci-firebase-notification"
                    }
                }
            }
        }'
    ```

    \
    This API will create a new adapter and will return the adapter id (Eg. 2a704e82-132e-41f2-9746-83e74550d2ea). We will use this adapter id later in conversation logic.
4. Create a new service [here](http://143.110.255.220:15003/console/data/default/schema/public/tables/service/browse) with the your token API.
5. Create a new user segment [here](http://143.110.255.220:15003/console/data/default/schema/public/tables/userSegment/browse) with the service created in step 4.
6.  Create a new conversation logic with adapter from step 3 and below logic.

    ```
    curl --location --request POST 'http://143.110.255.220:9999/admin/v1/conversationLogic/create' \
    --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "data": {
            "name": "Firebase Broadcast Logic",
            "transformers": [
                 {
                    "id": "774cd134-6657-4688-85f6-6338e2323dde",
                    "meta": {
                        "body": "Hello ${name}-${phoneNo}, Test Notification",
                        "type": "JS_TEMPLATE_LITERALS",
                        "params": [
                            "name",
                            "phoneNo"
                        ]
                    },
                    "type": "broadcast"
                }
            ],
            "adapter": "2a704e82-132e-41f2-9746-83e74550d2ea"
        }
    }'
    ```
7.  Create a new bot with the conversation logic from step 6, user segment from step 5.

    ```
    curl --location --request POST 'http://143.110.255.220:9999/admin/v1/bot/create' \
    --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "data": {
            "startingMessage": "Test Firebase Notification",
            "name": "Test Firebase Notification Bot",
            "users": ["cf085492-266f-46d1-841c-1b38e93bce3f"],
            "logic": [
                "03ec1ac2-76b3-496a-8184-9e9e9d01c048"
            ],
            "status": "enabled",
            "startDate": "2022-06-13",
            "endDate": "2023-06-13"
        }
    }'
    ```
8.  Hit the below api using the created bot id to send firebase notification to all registered FCM tokens.

    ```
        curl --location --request GET 'http://143.110.255.220:9999/campaign/start?campaignId=1ea5346d-8d98-4cf4-a470-9c234476f3d1'
    ```

### 3. Message Receipts Api

We have two apis that provides the option to add delivery & read receipts for the received message.

When a firebase notification is sent, some extra data required for the receipt apis are also sent in it. **externalId** (Message ID), **destAdd** (User ID) & **fcmDestAdd** (User FCM Token) will be sent in the notification data to be later used in the receipt apis.

**3.1. Delivery Receipt**

This api stores the delivery receipt against a message. To sent a delivery report use the below curl request with required details.

````
```
    curl --location --request POST 'http://143.110.255.220:8080/firebase/web' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "text": "",
        "from": "",
        "messageId": "",
        "eventType": "DELIVERED",
        "report": {
            "externalId": "70eb40a7eb4b4f10890b3f2f865e307e",
            "destAdd": "75********",
            "fcmDestAdd": "cytMfcuBgpembxn7GfNvDh:APA91bHsaJioCNAoWAZql1lxe4szwd74CJsPEVp1ngSCrzMcft6kB9ZrZnUZ9PUVU47kGiVSUAk70ayF55nzi8vU6jlWI4AGLkTo9ZBZnwMll0ZqVKioAQARvgE4GTUwIoTWAqASDFGH"
        }
    }'
```
````

**3.2. Read Receipt**

This api stores the read receipt against a message. To sent a read report use the below curl request with required details.

````
```
    curl --location --request POST 'http://143.110.255.220:8080/firebase/web' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "text": "",
        "from": "",
        "messageId": "",
        "eventType": "READ",
        "report": {
            "externalId": "70eb40a7eb4b4f10890b3f2f865e307e",
            "destAdd": "75********",
            "fcmDestAdd": "cytMfcuBgpembxn7GfNvDh:APA91bHsaJioCNAoWAZql1lxe4szwd74CJsPEVp1ngSCrzMcft6kB9ZrZnUZ9PUVU47kGiVSUAk70ayF55nzi8vU6jlWI4AGLkTo9ZBZnwMll0ZqVKioAQARvgE4GTUwIoTWAqASDFGH"
        }
    }'
```
````

### 4. History Api

UCI provides an api to fetch the conversation history against a bot or a user.

**4.1. User History**

Use below API to fetch the history against a bot. **** This API uses below required parameters to fetch the user history.

* **userId**: Phone number of user
* **provider**: Conversation provider. Eg. firebase (for in-app notification)
* **start & end date**: Conversations between start & end date.

```
curl --location --request GET 'http://143.110.255.220:8080/xmsg/history?userId=7597185708&provider=firebase&endDate=03-07-2022&startDate=01-07-2022'
```

**Limitation:** This API can provide maximum 1000 conversations.

**4.2. Bot History**

Use below API to fetch the history against a user. This API uses below required parameters to fetch the user history.

* **botId**: Conversation id
* **provider**: Conversation provider. Eg. firebase (for in-app notification)
* **start & end date**: Conversations between start & end date.

```
curl --location --request GET 'http://143.110.255.220:8080/xmsg/history/dump?provider=firebase&botId=1ea5346d-8d98-4cf4-a470-9c234476f3d1&endDate=19-07-2022&startDate=05-07-2022'
```

**Limitation:** This API can provide conversations for 15 days only.

These APIs will give the response in below format.

```
{
    "timestamp": "2022-07-20T09:24:06Z",
    "status": 200,
    "error": null,
    "message": null,
    "path": "/xmsg/history",
    "result": {
        "total": 1,
        "records": [
            {
                "messageState": "DELIVERED",
                "provider": "firebase",
                "botUuid": null,
                "payload": {
                    "text": "Hello surabhi-7597185708, Test Notification"
                },
                "channel": "web",
                "ownerOrgId": null,
                "id": "2004a970-f902-11ec-bf0b-19880ce512a2",
                "sessionId": null,
                "ownerId": null,
                "fromId": "7597185708",
                "userId": "admin",
                "timestamp": "2022-07-01T05:53:26.389"
            }
        ]
    }
}
```
