# History APIs

## 1. Overview

UCI provides APIs to fetch the conversation history against a bot or a user.

## **2. APIs**

### **2.1. User History**

Use below API to fetch the history against a bot. **** This API uses below required parameters to fetch the user history.

* **userId**: Phone number of user
* **provider**: Conversation provider. **Eg**. gupshup
* **start & end date**: Conversations between start & end date.

```
curl --location --request GET 'http://143.110.255.220:8080/xmsg/history?userId=75********&provider=gupshup&endDate=03-07-2022&startDate=01-07-2022'
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
                "messageId": "4e2aab66-012f-4e5b-92ff-9b6ad6c2859b",
                "messageState": "DELIVERED",
                "provider": "firebase",
                "botUuid": "694c3306-d27f-431c-87f4-ae37a41204a4",
                "payload": {
                    "text": "Hello surabhi-75********, Test Notification"
                },
                "channel": "web",
                "ownerOrgId": "ORG_001",
                "id": "2004a970-f902-11ec-bf0b-19880ce512a2",
                "sessionId": "1bc51202-2311-4831-aaee-cd481d9d916e",
                "ownerId": "abc51102-9814-4211-b71e-vd20vb9d8g57"
                "fromId": "75********",
                "userId": "admin",
                "timestamp": "2022-07-01T05:53:26.389"
            }
        ]
    }
}
```
