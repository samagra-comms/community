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

- ```public Mono<XMessage> convertMessageToXMsg(Object msg) // Converts API response object to XMessage ```

- ```public Mono<XMessage> processOutBoundMessageF(XMessage nextMsg) //Converts XMessage object to API response and call it.```

These methods are called by `inbound` and `outbound` services internally to process the incoming and outgoing messages.

All adapters with the above implementation will be valid. An example adapter can be found [here](https://github.com/samagra-comms/adapter/blob/release-4.8.0/src/main/java/com/uci/adapter/netcore/whatsapp/NetcoreWhatsappAdapter.java).

#### 1. Incoming content
Currently we accepts text/media/location/quickReplyButton/list messages. It should be implemeted according to the network provider(Netcore/Gupshup) [documentation](https://wadocs.pepipost.com/webhooks/overview/incoming-message).

To enable these, we have to add them in ```convertMessageToXMsg``` and convert these according to the [xMessage spec](XMessage Spec.md)


#### 2. Outgoing Content
We can also send text/media/location/quickReplyButton/list messages to user. As we are using the ODK forms, we use bind::stylingTags, bind::caption to show the media file or to show the select choices as list/quick reply buttons. There are a few constraints which will be applied with the quickReplyButton/list content.

Below are the few styling tags we have allowed for now.

- image //To send image file
- audio //To send audio file
- video //To send video file
- document //To send document file
- buttonsForListItems //To send choices as quick reply buttons
- list //To send choices as list

To send this type of content to user, adapter should implement it according to the network provider(Netcore/Gupshup) [documentation](https://wadocs.pepipost.com/webhooks/overview/incoming-message).


## 3. List of Adapter Implementations

- Gupshup-Whatsapp
- Netcore-Whatsapp

## 4. How to setup adapter

Setup the adapter repository to start working on it. Follow the steps given to add a new adapter & test it. 

1. Fork the below repository & clone it.

	```https://github.com/samagra-comms/adapter/```

2. Checkout to the recent branch.

3. Open the project in any IDE spring/intellij.

4. Follow [point 2](#2-creating-your-own-adapters) to create a new adapter.

5. Write test cases for inbound method ```convertMessageToXMsg``` & outbound method ```processOutBoundMessageF```.

6. A simple example to test the send text message is given in the [file](https://github.com/samagra-comms/adapter/blob/release-4.8.0/src/test/java/com/uci/adapter/netcore/whatsapp/NetcoreServiceTest.java).  


## 5. FAQs

To be updated based on incoming feedback. Feel free to write into tech@samagragovernance.in in case you have questions, feedback or want to know more!
