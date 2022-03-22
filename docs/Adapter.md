---
id: CommunicationAdapter
title: Conversation Channel Adapters
sidebar_label: Conversation Channel Adapters
---

## 1. Conversation Channel Adapters

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vise versa. Adapters are gateway to the external services and resposible to recieving user response and sending response to users. Thus the two major functions of Adapters are

- Convert API/webhook data from channel (and provider) to xMessages
- Convert xMessages back to API/webhook data format for the specific channel(and provider)

A simplified diagram of what adapters do is shown below. ![](https://samagra-development.github.io/docs/img/adapter.jpg)

## 2. Creating your own Adapters

The adapter and the inbound service are linked together as shown in the figure below. ![](https://samagra-development.github.io/docs/img/adapter-internal.jpg)

Similarly the adapter and the outbound service are linked it the following fashion. ![](https://samagra-development.github.io/docs/img/outbound.jpeg)

All adapters are named as `<ProviderName><ChannelName>Adapter`; for example GupshupWhatsappAdapter. Adapters should extend `AbstractProvider` and implement `IProvider`. Thus, it needs to implement the following methods:

- ```java
    public XMessage convertMessageToXMsg(Object msg) // Converts API response object to XMessage
  ```
- ```java
    public void processInBoundMessage(XMessage nextMsg) //Converts XMessage object to API response and call it.
  ```

These methods are called by `inbound` and `outbound` services internally to process the incoming and outgoing messages.

All adapters with the above implementation will be valid. An example adapter can be found [here]().

## 3. List of Adapter Implementations

- Gupshup-Whatsapp
- Gupshup-SMS

## 4. FAQs

To be updated based on incoming feedback. Feel free to write into tech@samagragovernance.in in case you have questions, feedback or want to know more!

## Coming Soon

- CDAC-SMS Adapter
- Gmail-Mail (Mailtrain)
- AmazonSES-Mail (Mailtrain)
