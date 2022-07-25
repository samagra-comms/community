# History APIs

## 1. Overview

UCI provides an API to fetch the conversation history against a bot or a user.

## **2. APIs**

### **2.1. User History**

Use below API to fetch the history against a bot. **** This API uses below required parameters to fetch the user history.

* **userId**: Phone number of user
* **provider**: Conversation provider. **Eg**. gupshup
* **start & end date**: Conversations between start & end date.

```
curl --location --request GET 'http://143.110.255.220:8080/xmsg/history?userId=7597185708&provider=gupshup&endDate=03-07-2022&startDate=01-07-2022'
```

**Limitation:** This API can provide maximum 1000 conversations.

### **2.2. Bot History**

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
