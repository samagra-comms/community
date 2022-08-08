# UCI Basics

## 1. Overview

UCI is a multi-channel communication management engine, designed to be extremely modular and configurable.

UCI has a total of 9 repositories which are being used to process the bot messages from inbound to outbound. We have 5 main repositories who will process the messages from receiving the messages from channel(Sms/Whatsapp), converting the message to xmessage, authenticating the user, producing the next message, converting it back to message format for channel and sending it to the channel(Sms/Whatsapp).&#x20;

The other repositories are the helper repositories with each doing different jobs.

## 2. Brief Description

A brief description about the repositories included are listed below.

### **2.1. Utils**

Utils communicate with the federation service via apis. It holds all the utility/helper classes which will be used in other repositories.

### **2.2. Message Rosa**

Message Rosa holds all the core models for XMessage & its property fields.

### **2.3. Dao**

Dao holds the dao & repository classes for XMessage.

### **2.4. Adapter**

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vice versa. Adapters are gateway to the external services and are responsible for receiving user response and sending the response to users. [Click here](adapters/) to see the detailed documentation of adapter.

### **2.5. Inbound**

Inbound receives the messages from a channel, and uses the channel adapter to convert it to XMessage format. Once the message is converted, it will be pushed to the kafka topic, the orchestrator will listen to this topic to further process it.

### **2.6. Orchestrator**

Orchestrator authenticates & processes the user data from whom the message is received. The XMessage will then be pushed to the transformers topic based on the conversation configuration, the corresponding transformer(transformer/broadcast-transformer) will listen to this topic to further process it.&#x20;

### **2.7. Transformer**

This is a ODK transformer, which transforms the previous xMessage from the user to one that needs to be sent next. It is a microservice that returns a new xMessage based on the previous user action, which will then be shown to the user.&#x20;

The XMessage will be pushed to the process-outbound topic, orchestrator listens to this topic and will further process it to & send it to outbound topic. [Click here](transformers/) to see the detailed documentation of adapter.

### **2.8. Broadcast Transformer**

Broadcast transformer transforms the single message to multiple messages for each user segment associated with a conversation. It is a micro service that generated multiple XMessage and push it to process-outbound topic, orchestrator listens to this topic and will further process it to & send it to outbound topic.&#x20;

### **2.9. Outbound**

Outbound converts the xMessage to the one that will be sent to the channel(sms/whatsapp). It will then be sent to the network provider(Netcore/Gupshup) who will send it to the channel.&#x20;

## 3. Basics

To have a more deeper understanding of UCI, we have mentioned a few features included & specification here.

* [XMessage Specification](xmessage-specification.md)
* [Transformers](transformers/)
* [Adapters](adapters/)
* [User Segment](user-segment.md)
* [Schema Overview](schema-overview/)

