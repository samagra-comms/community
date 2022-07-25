# Adapters

## 1. Conversation Channel Adapters

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vise versa. Adapters are gateway to the external services and resposible to recieving user response and sending response to users. Thus the two major functions of Adapters are

* Convert API/webhook data from channel (and provider) to xMessages
* Convert xMessages back to API/webhook data format for the specific channel(and provider)

A simplified diagram of what adapters do is shown below.

![](../../../../.gitbook/assets/adapter.jpg)

## 2. How to works

The adapter and the inbound service are linked together as shown in the figure below.

![](../../../../.gitbook/assets/adapter-internal.jpg)

Similarly the adapter and the outbound service are linked in the following fashion.

![](../../../../.gitbook/assets/outbound.jpeg)

## 3. List of Existing Adapters

* Gupshup-Whatsapp
* Netcore-Whatsapp
* UCI Web Channel
* [Firebase Adapter](firebase-notification-adapter.md)

## 4. How to create an adapter

[Click here](../../contribution-guide/create-an-adapter.md) to see how to create your own adapter.
