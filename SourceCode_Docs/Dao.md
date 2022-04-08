# Dao
Dao holds the dao & repository classes for XMessage.

## 1. XMessageDAO

XMessageDAO is basically dao structure for XMessage. It contains all the field of XMessage which will be stored in the database.\
The fields include :

- **id**: Auto generated ID for each XMessage to identify and distinguish each XMessage on database.

- **userId**: Refers to Unique User ID which will be used to authenticate user.

- **fromId** : Refers to the Unique form which contain Conversation Logic for current message.

- **channel**: Describe the channel from where message is coming (eg: WhatsApp / Telegram / SMS).

- **provider**: Describe the service provider being used for sending / receving messages from channel.

- **timestamp**: Time when the message was sent/generated.
- messageState

- **xMessage**: XMessage Payload converted in String format.

- **messageId**: Message ID is unique Key comes along with each message to identify and distinguish all messages.

- other than these XMessageDAO have more fields which include : app, auxData, replyId, causeId.

## 2. XMessageRepository

XMessageRepository contains all the data retrival queries for XMessageDAO. We use these methods to get desirable data from database, via diffrent parameters.

## 3. XMessageDAOUtils

XMessageDAOUtils is a util class to filter-out data of XMessage acording to XMessageDAO and store it on database.