# Unified Communications Interface (UCI)

- [Overview](#1-overview)
- [Project Structure](#2-project-structure)  
    - [Utils](#21-utils)
    - [Message Rosa](#22-message-rosa)
    - [Dao](#23-dao)
    - [Adapter](#24-adapter)
    - [Inbound](#25-inbound)
    - [Orchestrator](#26-orchestrator)
    - [Transformer](#27-transformer)
    - [Outbound](#28-outbound)
    - [UCI Admin](#29-uci-admin) 


## 1. Overview

Unified Communications Interface (UCI) is a system that powers governments to create and manage conversations with citizens and with its own officials. Through UCI governments can seamlessly setup simple and complex conversations using a multi-channel approach. UCI aims to democratize the use of different communication channels such as WhatsApp, Telegram, SMS, email for governance use cases through a standard configurable manner that is reusable and scalable across all governance use cases.

## 2. Project Structure

### 2.1 Utils :
Utils communicate with the [federation service](#29-uci-admin) via apis. It holds all the utility/helper classes which will be used in other repositories. Click to view the [Repository](https://github.com/samagra-comms/utils)

### 2.2 Message Rosa: 
Message Rosa holds all the core models for XMessage & its property fields. Click to view the [Repository](https://github.com/samagra-comms/transformer)

### 2.3 Dao :
Dao holds the dao & repository classes for XMessage. Click to view the [Repository](https://github.com/samagra-comms/dao)

### 2.4 Adapter :
Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vice versa. Adapters are gateway to the external services and are responsible for receiving user response and sending the response to users. Click to view the [Repository](https://github.com/samagra-comms/adapter)

### 2.5 Inbound :
Inbound receives the messages from a channel, and uses the channel adapter to convert it to XMessage format. Once the message is converted, it will be pushed to the kafka topic, the orchestrator will listen to this topic to further process it. Click to view the [Repository](https://github.com/samagra-comms/inbound).

### 2.6 Orchestrator :
Orchestrator authenticates & processes the user data from whom the message is received. The XMessage will then be pushed to the kafka topic, the transformer will listen to this topic to further process it. Click to view the [Repository](https://github.com/samagra-comms/orchestrator)

### 2.7 Transformer :
Transformers transforms the previous xMessage from the user to one that needs to be sent next. It is a microservice that returns a new xMessage based on the previous user action, which will then be shown to the user. The XMessage will be pushed to the kafka topic, the outbound will listen to this topic to further process it. Click to view the [Repository](https://github.com/samagra-comms/transformer)

### 2.8 Outbound :
Outbound converts the xMessage to the one that will be sent to the channel(sms/whatsapp). It will then be sent to the network provider(Netcore/Gupshup) who will send it to the channel. Click to view the [Repository](https://github.com/samagra-comms/outbound)

### 2.9 UCI Admin:
UCI Admin provides the apis to fetch the bot related data. UCI Admin connects with the database that holds the bot related data. Click to view the [Repository](https://github.com/samagra-comms/uci-apis)

