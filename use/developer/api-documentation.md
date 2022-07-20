# API Documentation

## 1. Conversation History

UCI provides an API to fetch the conversation history against a bot or a user.

**1.1. User History**

Use below API to fetch the history against a bot. **** This API uses below required parameters to fetch the user history.

* **userId**: Phone number of user
* **provider**: Conversation provider. **Eg**. gupshup
* **start & end date**: Conversations between start & end date.

```
curl --location --request GET 'http://143.110.255.220:8080/xmsg/history?userId=7597185708&provider=gupshup&endDate=03-07-2022&startDate=01-07-2022'
```

**Limitation:** This API can provide maximum 1000 conversations.

**1.2. Bot History**

Use below API to fetch the history against a user. This API uses below required parameters to fetch the user history.

* **botId**: Conversation id
* **provider**: Conversation provider. **Eg**. gupshup
* **start & end date**: Conversations between start & end date.

```
curl --location --request GET 'http://143.110.255.220:8080/xmsg/history/dump?provider=gupshup&botId=d655cf03-1f6f-4510-acf6-d3f51b488a5e&endDate=19-07-2022&startDate=05-07-2022'
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

### 2. Send Direct Message

Send direct messages from outbound service. This api used below parameters to send message to devices.

* **adapterId**: Id of adapter added for provider & channel **Eg**. Gupshup-Whatsapp/Netcore-Whatsapp etc.
* **to.userID:** Phone number of user
* **to.deviceType**: device type of user **Eg**. PHONE
* **to.meta.fcmToken**: FCM token of user for firebase notifications. It is required for firebase notifications.
* **payload.text**: **** Text to be sent to user
* **payload.media.url**: URL of image to be sent to user (In case of media messages)
* **payload.media.category**: category of media **Eg**. IMAGE\_URL. \
  _Available categories:_ IMAGE_URL, AUDIO\_URL, VIDEO\_URL, DOCUMENT\_URL_
* **payload.media.caption:** Caption to be sent with media file

Below are some curl requests for sending messages.

**2.1 Netcore Whatsapp Message**

* **Send text message to whatsapp number**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307323",
    "to": {
        "userID": "7597185708",
        "deviceType": "PHONE"
    },
    "payload": {
        "text": "Hello there!"
    }
}'
```

* **Send media message  to whatsapp number**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307323",
    "to": {
        "userID": "7597185708",
        "deviceType": "PHONE"
    },
    "payload": {
        "media": {
            "url": "https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885_960_720.jpg",
            "category": "IMAGE_URL",
            "text": "Image Caption"
        }
    }
}'
```

**2.2 Gupshup Whatsapp Message**

* **Send text message to whatsapp number**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307324",
    "to": {
        "userID": "7597185708",
        "deviceType": "PHONE"
    },
    "payload": {
        "text": "Hello there!"
    }
}'
```

* **Send media message to whatsapp number**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307324",
    "to": {
        "userID": "7597185708",
        "deviceType": "PHONE"
    },
    "payload": {
        "media": {
            "url": "https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885_960_720.jpg",
            "category": "IMAGE_URL",
            "text": "Image Caption"
        }
    }
}'
```

**2.3 Firebase Web Message**

* **Send message to device**&#x20;

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "2a704e82-132e-41f2-9746-83e74550d2ea",
    "to": {
        "userID": "7597185708",
        "deviceType": "PHONE",
        "meta": {
            "fcmToken": "cytMfcuBgpembxn7GfNvDh:APA91bHsaJioCNAoWAZql1lxe4szwd74CJsPEVp1ngSCrzMcft6kB9ZrZnUZ9PUVU47kGiVSUAk70ayF55nzi8vU6jlWI4AGLkTo9ZBZnwMll0ZqVKioAQARvgE4GTUwIoTWAqVUYGMN"
        }
    },
    "payload": {
        "text": "Testing"
    }
}'
```

****
