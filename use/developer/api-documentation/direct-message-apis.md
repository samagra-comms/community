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

## **2. APIs**

### **2.1 Netcore Whatsapp Message**

* **Send text message to whatsapp number**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307323",
    "to": {
        "userID": "75********",
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
        "userID": "75********",
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

### **2.2 Gupshup Whatsapp Message**

* **Send text message to whatsapp number**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "44a9df72-3d7a-4ece-94c5-98cf26307324",
    "to": {
        "userID": "75********",
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
        "userID": "75********",
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

### **2.3 Firebase Web Message**

* **Send message to device**&#x20;

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "2a704e82-132e-41f2-9746-83e74550d2ea",
    "to": {
        "userID": "75********",
        "deviceType": "PHONE",
        "meta": {
            "fcmToken": "cytMfcuBgpembxn7GfNvDh:APA91bHsaJioCNAoWAZql1lxe4szwd74CJsPEVp1ngSCrzMcft6kB9ZrZnUZ9PUVU47kGiVSUAk70ayF55nzi8vU6jlWI4AGLkTo9ZBZnwMll0ZqVKioAQARvgE4GTUwIoTWAqVUYGMN",
            "fcmClickActionUrl": "http://www.example.com"
        }
    },
    "payload": {
        "text": "Testing"
    }
}'
```

**Note:** Param **fcmClickActionUrl** is being used to mention the url to open on click ot fcm notification. This url should have the same domain as the one on which the fcm tokens are registered, else the url will not work.

### **2.4 Gupshup SMS**

* **Send message to phone**

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "64036edb-e763-44b1-99b8-37b6c7b292c5",
    "to": {
        "userID": "75********",
        "deviceType": "PHONE"
    },
    "payload": {
        "text": "Kindly note your OTP @__123__@. Submission of the OTP will be taken as authentication that you have personally verified and overseen the distribution of smartphone to the mentioned student ID of your school. Thank you! - Samagra Shiksha, Himachal Pradesh"
    }
}'
```

### **2.4 CDAC SMS**

* **Send message to phone**

For sending message to phone number via CDAC, a templateId which is approved by CDAC is required and the payload text should have content of this template body.

```
curl --location --request POST 'http://143.110.255.220:9090/message/send' \
--header 'Content-Type: application/json' \
--data-raw '{
    "adapterId": "4e0c568c-7c42-4f88-b1d6-392ad16b8546",
    "to": {
        "userID": "75********",
        "deviceType": "PHONE",
        "meta": {
            "templateId": "1007180838266527905"
        }
    },
    "payload": {
        "text": "प्रिय Sikha,मैं आपको होमवर्क के रूप में कुछ पाठ्य-सामग्री साझा कर रहा/रही हूं। कृपया उसे देखें और पूरा करें और उसके साथ अपनी शंकाओं पर चर्चा करें। इस लिंक पर क्लिक करें - http://www.google.com धन्यवाद! HPGOVT"
    }
}'
```
