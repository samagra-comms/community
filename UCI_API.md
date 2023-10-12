1. Import the following file to POSTMAN :
    * Samagra - UCI Apis - Campaign Service.postman_environment 
    * Samagra - UCI Simplified.postman_collection

2. Select enviorment "Samagra - UCI Apis - Campaign Service"

3. ODK Form Upload :
    * Hit below API with given form data.
    * Hit POST : {{base_url}}/admin/v1/forms/upload  
        ```
        |   Key     |       Value                |  
        |----------------------------------------|  
        |   form    |   Upload ODK XML Form File |
        ```

    * Sample Response :
        ```
        {
            "ts": "2022-03-11T11:12:13.808Z",
            "params": {
                "resmsgid": "1a929610-a12c-11ec-bfbe-8dd58ab29de6",
                "msgid": null,
                "status": "successful",
                "err": null,
                "errmsg": null
            },
            "responseCode": "OK",
            "result": {
                "data": "vF-rozgar-candidate-Mar09-v3"
            }
        }
        ```


4. Create a Conversation Logic
    * Hit below API with given JSON to create new bot.

    * Hit POST : {{base_url}}/admin/v1/conversationLogic/create
        ```
        {
            "data": {
                "name": "CLName",                      // Unique Conversation Logic Name
                "transformers": [
                    {
                        "id": "Transformer-ID",         // Transformer-ID
                        "meta": {
                            "form": "https://hosted.my.form.here.com",
                            "formID": "Form-ID"         // Form-ID
                        }
                    }
                ],
                "adapter": "Adapter-ID"                 // Adapter-ID
            }
        }
        ```

    * Response of this API will provide Conversation-Logic-ID which will be used to create new bot.

    * Sample Request :
        ```
        {
            "data": {
                "name": "STest Form6",
                "transformers": [
                    {
                        "id": "bbf56981-b8c9-40e9-8067-468c2c753659",
                        "meta": {
                            "form": "https://hosted.my.form.here.com",
                            "formID": "global_bot_2"
                        }
                    }
                ],
                "adapter": "44a9df72-3d7a-4ece-94c5-98cf26307324"
            }
        }
        ```

    * Sample Response :
        ```
        {
            "ts": "2022-03-09T11:36:05.547Z",
            "params": {
                "resmsgid": "1b20f7b0-9f9d-11ec-98a6-5f87a368354d",
                "msgid": null,
                "status": "successful",
                "err": null,
                "errmsg": null
            },
            "responseCode": "OK",
            "result": {
                "data": {
                    "transformers": "[{\"id\":\"bbf56981-b8c9-40e9-8067-468c2c753659\",\"meta\":{\"form\":\"https://hosted.my.form.here.com\",\"formID\":\"global_bot_2\"}}]",
                    "adapter": "44a9df72-3d7a-4ece-94c5-98cf26307324",
                    "name": "STest Form6",
                    "id": "ee426a47-e690-4dae-ae89-d74ccbfdba40"
                }
            }
        }
        ```


5. create a bot :
    * Hit below API with given JSON to create new bot.

    * Hit POST : {{base_url}}/admin/v1/bot/create
        ```
        {
            "data": {
                "startingMessage": "Starting Message",  // Unique Starting Message
                "name": "BotName",                      // Unique Name
                "users": [],
                "logic": [
                    "Logic-ID"                          // Conversation-Logic-ID
                ],
                "status": "enabled",
                "startDate": "2021-11-11",              // Start-Date
                "endDate": "2021-11-11"                 // End-Date
            }
        }
        ```

    * Sample Request :
        ```
        {
            "data": {
                "startingMessage": "Hi Test Bot 36",
                "name": "STest 26",
                "users": [],
                "logic": [
                    "ee426a47-e690-4dae-ae89-d74ccbfdba40"
                ],
                "status": "enabled",
                "startDate": "2021-11-11",
                "endDate": "2021-11-11"
            }
        }
        ```

    * Sample Response :
        ```
        {
            "ts": "2022-03-09T11:39:22.562Z",
            "params": {
                "resmsgid": "908f1220-9f9d-11ec-98a6-5f87a368354d",
                "msgid": null,
                "status": "successful",
                "err": null,
                "errmsg": null
            },
            "responseCode": "OK",
            "result": {
                "data": {
                    "startingMessage": "Hi Test Bot 36",
                    "name": "STest 26",
                    "users": [],
                    "status": "enabled",
                    "startDate": "2021-11-11",
                    "endDate": "2021-11-11",
                    "logicIDs": [
                        "ee426a47-e690-4dae-ae89-d74ccbfdba40"
                    ],
                    "id": "8a8c26e5-6227-47a9-8e70-7b6ca90d14e5"
                }
            }
        }
        ```


    
