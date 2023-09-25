# Whatsapp Flow (Netcore)

## Types of Bots:

1. **Broadcast Bot:** This type of bot is used to send a single message to a large group of people. It doesn't expect a reply to the message.

2. **Conversation Bot:** This type of bot is designed for a two way conversation with a user. The conversation is regulated by an ODK form.

## Broadcast Flow

This is a 4 step process.

1. Create conversation logic: 

```bash
curl --location 'http://localhost:3002/admin/conversationLogic' \
--header 'admin-token: 4737ea8520bd454caabb7cb3d36e14bc1832c0d3f70a4189b82598670f11b1bd' \
--header 'Content-Type: application/json' \
--header 'Cookie: fusionauth.locale=en_US; fusionauth.sso=AgOat0GjncGOHhPpH_HuL9QQqnfMitd15O-ofS-uTcdA' \
--data '{
    "data": {
        "id": null,
        "name": "Netcore Demo",
        "description": "Netcore Demo",
        "transformers": [
            {
                "id": "774cd134-6657-4688-85f6-6338e2323dde",
                "meta": {
                    "body": "Hello ${name}-${phoneNo}, Netcore Test Notification Body demo dry run.",
                    "type": "broadcast",
                    "title": "Netcore Demo",
                    "params": [
                        "name",
                        "phoneNo"
                    ],
                    "templateType": "JS_TEMPLATE_LITERALS"
                }
            }
        ],
        "adapter": "44a9df72-3d7a-4ece-94c5-98cf26307323"
    }
}'
```

| Parameter      | Description                                                                                                 |
|----------------|-------------------------------------------------------------------------------------------------------------|
| id             | The ID of the conversation logic.             |
| name           | The name of the conversation logic.                                                                          |
| description    | The description of the conversation logic.                                                                   |
| transformers   | An array of transformer objects that define the message content and behavior.                             |
|                |   - id: The ID of the transformer.                                                                           |
|                |   - meta: An object containing metadata about the transformer.                                             |
|                |     - body: The message content.                                                                             |
|                |     - type: The type of message, e.g., "broadcast".                                                         |
|                |     - title: The title of the message.                                                                      |
|                |     - params: An array of parameter names used in the message template.                                   |
|                |     - templateType: The template type, e.g., "JS_TEMPLATE_LITERALS".                                       |
| adapter        | The ID of the adapter associated with the conversation logic.                                             |


This will create a conversation logic for the broadcast flow. We don't need an ODK form here as this is not a two-way conversation flow that is driven by ODK logic. The conversation logic for this type of bot only has a single broadcast message which is defined in the meta of the request.

2. Create user segment:

Creating a user segment is analogous to importing a list of users into UCI. The curl for this is given below.

```bash
curl --location 'http://localhost:3002/admin/user-segment' \
--header 'admin-token: 4737ea8520bd454caabb7cb3d36e14bc1832c0d3f70a4189b82598670f11b1bd' \
--header 'Content-Type: application/json' \
--data '{
    "name": "Netcore demo",
    "all": {
        "type": "get",
        "config": {
            "url": "http://localhost:9080/service/testUserSegment2",
            "type": "GET",
            "cadence": {
                "perPage": 1,
                "retries": 5,
                "timeout": 60,
                "concurrent": true,
                "pagination": true,
                "concurrency": 10,
                "retries-interval": 10
            },
            "pageParam": "page",
            "credentials": {},
            "totalRecords": 1
        }
    },
    "byID": {
        "type": "get",
        "config": {
            "url": "http://localhost:9080/service/testUserSegment2",
            "type": "GET",
            "cadence": {
                "perPage": 1,
                "retries": 5,
                "timeout": 60,
                "concurrent": true,
                "pagination": true,
                "concurrency": 10,
                "retries-interval": 10
            },
            "pageParam": "page",
            "credentials": {},
            "totalRecords": 1
        }
    },
    "byPhone": {
        "type": "get",
        "config": {
            "url": "http://localhost:9080/service/testUserSegment2",
            "type": "GET",
            "cadence": {
                "perPage": 1,
                "retries": 5,
                "timeout": 60,
                "concurrent": true,
                "pagination": true,
                "concurrency": 10,
                "retries-interval": 10
            },
            "pageParam": "page",
            "credentials": {},
            "totalRecords": 1
        }
    },
    "count": 0
}'
```
This creates a new user segment with the user list provided on `http://localhost:9080/service/testUserSegment2`.

| Parameter | Description                                                                                             |
|-----------|---------------------------------------------------------------------------------------------------------|
| name      | The name of the user segment.                                                                           |
| all       | Configuration for retrieving all users in the segment.                                                |
|           |   - type: Type of HTTP request (e.g., "get") for retrieving users.                                  |
|           |   - config: An object containing configuration settings for the HTTP request.                       |
|           |     - url: The URL to which the HTTP request will be sent to retrieve user data.                     |
|           |     - type: The type of HTTP request (e.g., "GET").                                                  |
|           |     - cadence: An object containing settings related to timing and concurrency of the requests.     |
|           |     - pageParam: The parameter name used for pagination in the URL's query parameters.              |
|           |     - credentials: An object containing credentials (if required) for making the request.           |
|           |     - totalRecords: A field indicating the total number of records in the user segment.             |
| byID      | Configuration for retrieving users by their IDs.                                                     |
| byPhone   | Configuration for retrieving users by their phone numbers.                                           |
| count     | A field indicating the count of users in the user segment.                                           |

The user list on the provided URL should be in the following format:

```json
{
  "data": [
    {
      "phoneNo": "<PHONE_NUMBER>",
      "name": "<USER_NAME>",
    }
  ]
}
```

| Parameter            | Description                                         |
|----------------------|-----------------------------------------------------|
| data.phoneNo         | The phone number of the user.                      |
| data.name            | The name of the user.                              |

These params are necessary for the flow to work. Apart from these params, any additional params which are defined while creating conversation logic should also be provided in this object.

### Note:
This user list definition is only valid for Whatsapp flow. Definitions will differ for different use cases.

3. Create a broadcast bot:

```bash
curl --location 'http://localhost:3002/admin/bot' \
--header 'admin-token: 4737ea8520bd454caabb7cb3d36e14bc1832c0d3f70a4189b82598670f11b1bd' \
--form 'botImage=@"/home/ryanwalker277/Pictures/Screenshot from 2023-08-08 15-10-09.png"' \
--form 'data="{
    \"data\": {
        \"startingMessage\": \"Hi Netcore demo\",
        \"name\": \"Hi Netcore demo\",
        \"users\": [\"1be4e149-3618-4135-84ac-f39c49d6f671\"],
        \"logic\": [
            \"2b16fd7b-6c87-4726-9035-c20400901a82\"
        ],
        \"status\": \"enabled\",
        \"startDate\": \"2023-03-01\",
        \"endDate\": \"2024-12-01\",
        \"ownerid\":\"8f7ee860-0163-4229-9d2a-01cef53145ba\",
        \"ownerorgid\":\"org01\",
        \"purpose\":\"Testing\",
        \"desc\":\"Netcore Broadcast Bot Desc\"
    }
}"'
```

| Parameter             | Description                                                   |
|-----------------------|---------------------------------------------------------------|
| botImage              | The image file of the bot to be uploaded.                     |
| data                  | A JSON object containing various configuration settings for the bot. |
| data.startingMessage  | The initial message of the bot to trigger the conversation flow. |
| data.name             | The name of the bot.                                          |
| data.users            | An array of user segments attached to the bot                 |
| data.logic            | Conversation logic associated with the bot.        |
| data.status           | The status of the bot ("enabled" or "disabled").              |
| data.startDate        | The start date of the bot's activity.                         |
| data.endDate          | The end date of the bot's activity.                           |
| data.ownerid          | The ID of the bot's owner. (Vault)                                    |
| data.ownerorgid       | The organization ID of the bot's owner. (Vault)                       |
| data.purpose          | A description of the purpose of the bot.                      |
| data.desc             | A description of the bot.                                     |


At the time of bot creation, it must be attached to a user segment. This is done by passing the segment id, which is a unique identifier for the user segement created in the previous step, in the users section of the request body. The segment id can be obtained from the response of the previous step. Add the conversation logic id to the body in logic section. 

4. Triggering the bot:

```bash
curl --location  'http://localhost:3002/campaign/start?campaignId=859ed55f-c276-4f38-ba72-5f08e1733705'
```

Replace the campaignId with the bot id given by the bot creation api.This will trigger a broadcast notification for all the users in that segment over whatsapp.

## Conversation Flow:

This is a 3 step process.

1. Upload ODK form:

```bash
curl --location 'http://localhost:3002/admin/form/upload' \
--header 'ownerId: 8f7ee860-0163-4229-9d2a-01cef53145ba' \
--header 'ownerOrgId: org1' \
--header 'admin-token: 4737ea8520bd454caabb7cb3d36e14bc1832c0d3f70a4189b82598670f11b1bd' \
--form 'form=@"/home/ryanwalker277/Music/WhatsappDemo/SampleOdkForm.xml"'
```

| Parameter      | Description                                         |
|----------------|-----------------------------------------------------|
| ownerID        | The ID of the owner of the form. (Vault)                   |
| ownerOrgId     | The organization ID of the owner of the form. (Vault)      |
| admin-token    | The admin token for authentication.                |
| form           | The XML file of the form to be uploaded.           |

Refrence to create an [ODK Form](https://docs.getodk.org/xlsform/)

In the conversation logic, ODK form becomes necessary as the whole conversation flow is determined by this.

2. Create conversation logic:

```bash
curl --location 'http://localhost:3002/admin/conversationLogic' \
--header 'admin-token: 4737ea8520bd454caabb7cb3d36e14bc1832c0d3f70a4189b82598670f11b1bd' \
--header 'Content-Type: application/json' \
--header 'Cookie: fusionauth.locale=en_US; fusionauth.sso=AgOat0GjncGOHhPpH_HuL9QQqnfMitd15O-ofS-uTcdA' \
--data '{
    "data": {
        "id": null,
        "name": "Netcore Demo",
        "description": "Necore Desc",
        "transformers": [
            {
                "id": "bbf56981-b8c9-40e9-8067-468c2c753659",
                "meta": {
                    "form": "https://hosted.my.form.here.com",
                    "formID": "List-Button-test-v5"
                }
            }
        ],
        "adapter": "44a9df72-3d7a-4ece-94c5-98cf26307323"
    }
}'
```

| Parameter      | Description                                         |
|----------------|-----------------------------------------------------|
| data           | An object containing the conversation logic data.  |
| data.id        | The ID of the conversation logic (null for new).   |
| data.name      | The name of the conversation logic.                |
| data.description | The description of the conversation logic.         |
| data.transformers.id | The ID of the transformer.                     |
| data.transformers.meta | An object containing metadata about the transformer. |
| data.transformers.meta.form | The URL of the form associated with the transformer. |
| data.transformers.meta.formID | The ID of the form associated with the transformer. |
| data.adapter   | The ID of the adapter associated with the logic.   |


Replace the formId with the id of the form uploaded. This returns a unique conversation logic id.

3. Create conversation bot:


```bash
curl --location 'http://localhost:3002/admin/bot' \
--header 'admin-token: 4737ea8520bd454caabb7cb3d36e14bc1832c0d3f70a4189b82598670f11b1bd' \
--form 'botImage=@"/home/ryanwalker277/Music/WhatsappDemo/boticon.jpeg"' \
--form 'data="{
    \"data\": {
        \"startingMessage\": \"Hi uci\",
        \"name\": \"Netcore Demo\" ,
        \"users\": [],
        \"logic\": [\"38dde1ef-d618-4440-adc7-34a7df885187\"],
        \"status\": \"enabled\",
        \"startDate\": \"2023-05-4\",
        \"endDate\": \"2025-12-01\"
    }
}"'
```

| Parameter            | Description                                           |
|----------------------|-------------------------------------------------------|
| botImage             | The image file of the bot to be uploaded.            |
| data                 | An object containing the bot data.                   |
| data.startingMessage | The initial message of the bot.                      |
| data.name            | The name of the bot.                                 |
| data.users           | An array of user IDs associated with the bot.        |
| data.logic           | Conversation logic associated with the bot.  |
| data.status          | The status of the bot ("enabled" or "disabled").     |
| data.startDate       | The start date of the bot's activity.                |
| data.endDate         | The end date of the bot's activity.                  |
| botImage             | The image file to be uploaded as the bot's image.    |

Notice that we are not passing the segment id here. That means this bot is not attached to any user segment. So, any user can interact with this bot by sending the starting message. 
