# Message-Rosa

Message Rosa holds all the core models for XMessage & its property fields.

## 1. XMessage

XMessage is a standard defined for the UCI ecosystem. The key objective of this is to make the internal functioning of UCI, independent of external factors viz. Communication channel and service provider.\
XMessage class in Message-Rosa majorly focuses on the following properties :

- **messageId**: Message-ID is a unique Key that comes along with each message to identify and distinguish all messages.

- **payload**: Payload contains all the message data which is separatly defined in [XMessage Payload](#2-xmessage-payload).

- **messageType**: This is enum which describe the type of message from one of (broadcast_text / text / hsm_with_button / hsm).

- **to - from**: These stores the information of sender and receiver that from where the message is coming and to where it needs to deliver.

- **channelURI**: Describe the channel from where message is coming (eg: WhatsApp / Telegram / SMS).

- **providerURI**: Describe the service provider being used for sending/receiving messages from the channel.

- **adapterId**: Refers to Adapter-ID used to convert from API/webhook data to XMessage, so with the same adapter the response XMessage would be changing in the original API/webhook data.

- **transformers**: contains the reference of transformers to be used for processing XMessage and generating the response.

- other than these major properties, XMessage have more properties which include: app, timestamp, userState, encryptionProtocol, messageState, lastMessageID, conversationStage, conversationLevel, thread, marshaller, context.


## 2. XMessage Payload

XMessagePayload is the body of a message, which contains all the data inside of a message.\
XMessagePayload has the following properties :

- **text**: contains text attached with the message.

- **stylingTag**: useful to indicate that message contains any media or list/button view.

- **media**: contains media data attached with the message, it will be _null_ if not applicable.

- **mediaCaption**: Caption to be attached with media, it will be _null_ if not applicable.

- **location**: contains location data attached with the message, it will _null_ if not applicable.

- **buttonChoices**: useful for sending a response with button choices, it will _null_ if not applicable.

- other than these XMessagePayload has more properties which include: flow, questionIndex, contactCard.