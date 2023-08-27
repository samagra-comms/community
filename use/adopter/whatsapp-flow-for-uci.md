# Whatsapp Flow (Netcore)

## Types of Bots:

1. **Broadcast Bot:** This type of bot is used to send a single message to a large group of people. It doesn't expect a reply to the message.

2. **Conversation Bot:** This type of bot is designed for a proper conversation flow with a user. It facilitates a two-way conversation that is regulated by the ODK form.

## Broadcast Flow

This is a 4 step process.

1. Create conversation logic: 

```bash
curl --location 'http://localhost:3002/admin/conversationLogic' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
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

This will create a conversation logic for the broadcast flow. We don't need an ODK form here as this is not a multi-message flow. The conversation logic for this type of bot only has a single broadcast message which is defined in the meta of the request.

2. Create user segment:

Creating a user segment is analogous to importing a list of users into UCI. The curl for this is given below.

```bash
curl --location 'http://localhost:3002/admin/user-segment' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
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

3. Create a broadcast bot:

```bash
curl --location 'http://localhost:3002/admin/bot' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
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

At the time of bot creation, it must be attached to a user segment. This is done by passing the segment id in the users section of the request body. Add the conversation logic id to the body in logic section. 

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
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
--form 'form=@"/home/ryanwalker277/Music/WhatsappDemo/SampleOdkForm.xml"'
```

In the conversation flow, ODK form becomes necessary as the whole conversation flow is determined by this.

2. Create conversation logic:

```bash
curl --location 'http://localhost:3002/admin/conversationLogic' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
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

Replace the formId with the id of the form uploaded. This returns a unique conversation logic id.

3. Create conversation bot:


```bash
curl --location 'http://localhost:3002/admin/bot' \
--header 'admin-token: dR67yAkMAqW5P9xk6DDJnfn6KbD4EJFVpmPEjuZMq44jJGcj65' \
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

If you notice, we are not passing the segment id here. That means this bot is not attached to any user segment. So, any user can interact with this bot by sending the starting message. 
