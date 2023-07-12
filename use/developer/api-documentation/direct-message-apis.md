# Direct Message APIs

## 1. Overview

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

This API will give you response in below format. If the api returns a succes response, it will include a message id in result.

```
{    
    "timestamp": "2022-08-04T11:51:51Z",    
    "status": 200,    
    "error": null,    
    "message": "Message sent.",    
    "path": "/message/send",    
    "result": {        
        "messageId": "2a03363a-7752-4a19-b7f7-e3a1e1423ae8"    
    }
}
```

Below are some curl requests for sending messages.

## 2. APIs

### 2.1 Netcore Whatsapp Message

* **Send text message to whatsapp number**

    ```
    curl --location --request POST 'OUTBOUND:PORT/message/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307323",
        "to": {
            "userID": "97********",
            "deviceType": "PHONE"
        },
        "payload": {
            "text": "Hello there!"
        }
    }'
    ```

* **Send media message  to whatsapp number**

    ```
    curl --location --request POST 'OUTBOUND:PORT/message/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307323",
        "to": {
            "userID": "97********",
            "deviceType": "PHONE"
        },
        "payload": {
            "media": {
                "url": "@{IMAGE_PATH/URL}",
                "category": "IMAGE",
                "text": "Image Caption"
            }
        }
    }'
    ```

### 2.2 Gupshup Whatsapp Message

* **Send text message to whatsapp number**

    ```
    curl --location --request POST 'OUTBOUND:PORT/message/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307324",
        "to": {
            "userID": "97********",
            "deviceType": "PHONE"
        },
        "payload": {
            "text": "Hello there!"
        }
    }'
    ```

* **Send media message to whatsapp number**

    ```
    curl --location --request POST 'OUTBOUND:PORT/message/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307323",
        "to": {
            "userID": "97********",
            "deviceType": "PHONE"
        },
        "payload": {
            "media": {
                "url": "@{IMAGE_PATH/URL}",
                "category": "IMAGE",
                "text": "Image Caption"
            }
        }
    }'
    ```

### 2.3 Firebase Web Message

* **Send message to device**&#x20;

    The **data** field of the payload contains all the data that the backend might want to send to the user along with the message.

    ```
        curl --location --request POST 'http:/OUTBOUND:PORT/message/send' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "adapterId": "21f1d315-55cf-44e3-8355-4743d6519649",
            "to": {
                "userID": "97********",
                "deviceType": "PHONE"
            },
            "payload": {
                "text": "Testing",
                "data": [
                    {
                        "key": "fcmToken",
                        "value": "myFCMToken"
                    },
                    {
                        "key": "fcmClickActionUrl",
                        "value": "nipunlakshya://chatbot"
                    },
                    {
                        "key": "botDesc",
                        "value": "This is testing description"
                    },
                    {
                        "key": "botId",
                        "value": "This is testing bot id"
                    }
                ]
            }
        }'
    ```

**Note:** Param **fcmClickActionUrl** is being used to mention the url to open on click ot fcm notification. This url should have the same domain as the one on which the fcm tokens are registered, else the url will not work.

### 2.4 Gupshup SMS

* **Send message to phone**

    ```
    curl --location --request POST 'OUTBOUND:PORT/message/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "adapterId": "64036edb-e763-44b1-99b8-37b6c7b292c5",
        "to": {
            "userID": "97********",
            "deviceType": "PHONE"
        },
        "payload": {
            "text": "Hi user"
        }
    }'
    ```

### 2.4 CDAC SMS

* **Send message to phone**

    For sending message to phone number via CDAC, a **templateId** which is approved by CDAC is required and the payload text should have content of this template body.

    ```
    curl --location --request POST 'OUTBOUND:PORT/message/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "adapterId": "77164079-579a-4a70-b578-7dfe2afb677a",
        "to": {
            "userID": "97*********",
            "deviceType": "PHONE",
            "meta": {
                "templateId": "1007180838266527905"
            }
        },
        "payload": {
            "text": "Hi User"
        }
    }'
    ```
