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
2. Create a new service [here](http://143.110.255.220:15003/console/data/default/schema/public/tables/service/browse) with the your token API.
3. Create a new user segment [here](http://143.110.255.220:15003/console/data/default/schema/public/tables/userSegment/browse) with the service created in step 2.
4.  Create a new conversation logic with below logic

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
5.  Create a new bot with the conversation logic from step 4, user segment from step 3.

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
6. Share your Firebase Project service key for which these FCM tokens are registered with [UCI](firebase-notification-adapter.md#3.-contact-the-administrator). We will put this key in settings to allow the firebase notifications to be sent.
7.  Hit the below api using the created bot id to send firebase notification to all registered FCM tokens.

    ```
    curl --location --request GET 'http://143.110.255.220:9999/campaign/start?campaignId=1ea5346d-8d98-4cf4-a470-9c234476f3d1'
    ```

### 3. Contact the administrator

Please write to the Maintainer - Chakshu (chakshu@samagragovernance.in), and cc - Saket (saket@samagragovernance.in), Sukhpreet (sukhpreet@samagragovernance.in)
