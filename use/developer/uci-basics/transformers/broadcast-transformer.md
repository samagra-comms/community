# Broadcast Transformer

## Overview

Broadcast transformer converts a single message to multiple messages. Orchestrator send a list of user with there messages to be sent in a single XMessage. This transformer will generate mutiple xMessages based on the list of users & messages.&#x20;

It then pushes these messages to process outbound topic. Orchestrator gets to decide if any other change is required for these messages or to send it to directly to user via outbound.&#x20;

## Configuration

A bot will work with a broadcast transformer when we mention the type of the transformer as **broadcast.** When a bot is added, certain configuration will be added with this bot. This configuration needs to be changed for broadcast transformer. A sample broadcast configuration is given below

```
{
    "id": "774cd134-6657-4688-85f6-6338e2323dde",
    "meta": {
        "body": "Hi Sanchita! ${name} has submitted an air travel request for ${phoneNo}.",
        "type": "JS_TEMPLATE_LITERALS",
        "params": ["name", "phoneNo"]
    },
    "type": "broadcast"
}
```

Below are the brief details of each parameters included.

1. **id**: id of broadcast transformer
2. **meta:** Data related to message that will be to users.
   1. **body**: Message template text with parameters in it. Parameters should be included in the template in as **"${name}"**.
   2. **type**: Type of message template. **Eg**. JS\_TEMPLATE\_LITERALS
   3. **params**: Array of parameters provided in user segment. These parameter's values from user segment will be replaced in the message template text.&#x20;
3. &#x20;**type**: Type of transformer. **Eg**. broadcast

This configuration will be used when create a conversation logic. [Click here](../../api-documentation/bot-setup-apis.md#2.2-create-a-conversation-logic) to see the conversation logic api.

## User segment

User segment needs to be defined to broadcast a message to users. This user segment must have a **phoneNo** parameter in it. The user segment can have multiple parameters in it, these parameters will be used later to be replaced in the message template text before sending the message to users.

Below is a sample of user segment.

```
{
    "data": [
        {
            "id": "1",
            "phoneNo": "9877777777",
            "name": "John",
            "url": "http://google.com"
        },
        {
            "id": "2",
            "phoneNo": "9866666666",
            "name": "Mike",
            "url": "http://google.com"
        }
    ]
}
```

&#x20; &#x20;
