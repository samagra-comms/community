# Outbound

Outbound converts the xMessage to the one that will be sent to the channel(sms/whatsapp). It will then be sent to the network provider(Netcore/Gupshup) who will send it to the channel.

For this Outbound uses `OutboundKafkaController` class which listen from kafka topic through `reactiveKafkaReceiver` and receive XMessage Response generated from Transformer.\
Now this XMessage is converted back to Webhook/API data via relevent adapter and sent to network provider (Netcore/GupShup)

