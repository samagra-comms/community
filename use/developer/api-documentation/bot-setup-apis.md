# Bot Setup APIs

## 1. Overview

UCI provides a list of apis that will help you to configure bots for conversation or broadcasting etc.&#x20;

## 2. APIs

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
curl --location --request POST 'http://143.110.255.220:9999/admin/v1/adapter/create' \
    --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
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

This API will create a new adapter and will return the adapter id (Eg. 2a704e82-132e-41f2-9746-83e74550d2ea). We can use this adapter id later in [create conversation logic](bot-setup-apis.md#2.3-create-a-conversation-logic) api.

### 2.2 Upload ODK Form

For using ODK transformer, we will first have to upload a ODK form. Follow below steps to upload a ODK form.

*   Convert a ODK Excel form to XML form using [Link](https://getodk.org/xlsform/).

    [Sample ODK Excel Form](https://github.com/samagra-comms/docker-deploy/blob/main/media/List-QRB-Test-Bot.xlsx)
*   Upload this XML from using curl.

    ```
    curl --location --request POST 'http://143.110.255.220:9999/admin/v1/forms/upload' \
    --header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
    --form 'form=@"{PATH_OF_ODK_FORM}"'
    ```

    [Sample ODK XML Form](https://github.com/samagra-comms/docker-deploy/blob/main/List-QRB-Test-Bot.xml)

    **Response**: The API will return a form id. Use this form id to create conversation logic API. Form id E.g. **List-Button-test-v1**

    ```
    {
        "ts": "2022-05-24T13:46:06.640Z",
        "params": {
            "resmsgid": "dc586de0-db67-11ec-ae84-fbd67a9c1174",
            "msgid": null,
            "status": "successful",
            "err": null,
            "errmsg": null
        },
        "responseCode": "OK",
        "result": {
            "data": "List-Button-test-v1"
        }
    }    
    ```

### 2.3 Add a Conversation Logic

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
curl --location --request POST 'http://143.110.255.220:9999/admin/v1/conversationLogic/create' \
--header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
--header 'Content-Type: application/json' \
--data-raw '{
    "data": {
        "name": "UCI demo bot logic",
        "transformers": [
            {
                "id": "bbf56981-b8c9-40e9-8067-468c2c753659",
                "meta": {
                    "form": "https://hosted.my.form.here.com",
                    "formID": "List-Button-test-v1"
                }
            }
        ],
        "adapter": "44a9df72-3d7a-4ece-94c5-98cf26307323"
    }
}'
```

**Response**: It will return a conversation logic id, use it in create bot api. Eg. **92f7b965-4118-4ddc-9c7d-0bc0f77092db**

```
{
    "ts": "2022-05-24T13:48:06.407Z",
    "params": {
        "resmsgid": "23b94970-db68-11ec-ae84-fbd67a9c1174",
        "msgid": null,
        "status": "successful",
        "err": null,
        "errmsg": null
    },
    "responseCode": "OK",
    "result": {
        "data": {
            "transformers": "[{"id":"bbf56981-b8c9-40e9-8067-468c2c753659","meta":{"form":"https://hosted.my.form.here.com/%22,/%22formID/%22:/%22List-Button-test-v1/%22%7D%7D]",
            "adapter": "44a9df72-3d7a-4ece-94c5-98cf26307323",
            "name": "UCI demo bot logic",
            "id": "92f7b965-4118-4ddc-9c7d-0bc0f77092db"
        }
    }
}
```

### 2.4 Add a bot

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
curl --location --request POST 'http://143.110.255.220:9999/admin/v1/bot/create' \
--header 'admin-token: EXnYOvDx4KFqcQkdXqI38MHgFvnJcxMS' \
--header 'Content-Type: application/json' \
--data-raw '{
    "data": {
        "startingMessage": "Hi Test Bot",
        "name": "Test Bot",
        "users": [],
        "logic": [
            "c556dfc8-5dd3-477c-83bb-65d234c4d223" // Get this id from Create a conversation logic api.
        ],
        "status": "enabled",
        "startDate": "2022-05-24",
        "endDate": "2023-05-24"
    }
}'
```

**Response**: This api will return a bot id & other bot information. Use the starting message (Eg. **Hi Test Bot**) from here to start conversation with a bot.

```
{
    "ts": "2022-05-24T13:49:15.292Z",
    "params": {
        "resmsgid": "4cc874d0-db68-11ec-ae84-fbd67a9c1174",
        "msgid": null,
        "status": "successful",
        "err": null,
        "errmsg": null
    },
    "responseCode": "OK",
    "result": {
        "data": {
            "startingMessage": "Hi Test Bot",
            "name": "Test Bot",
            "users": [],
            "status": "enabled",
            "startDate": "2022-05-24",
            "endDate": "2023-05-24",
            "logicIDs": [
                "92f7b965-4118-4ddc-9c7d-0bc0f77092db"
            ],
            "id": "9f0b1401-44d2-46be-83bd-7cbd5014f899"
        }
    }
```
