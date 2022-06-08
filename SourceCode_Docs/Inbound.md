# Inbound

Inbound receives the messages from a channel, and uses the channel adapter to convert it to XMessage format. Once the message is converted, it will be pushed to the kafka topic, the orchestrator will listen to this topic to further process it.

## 1. Netcore Whatsapp Converter

- `NetcoreWhatsappConverter` class accept incoming data from WhatsApp in Netcore Webhook/API data.
- this data is sent to `XMsgProcessingUtil` class to process further checks  and validations.
- `XMsgProcessingUtil` class use `NetcoreWhatsappAdapter` to convert Webhook/API data into XMessage format.

## 2. GupShup Whatsapp Converter

- `GupShupWhatsappConverter` class accept incoming data from WhatsApp in GupShup Webhook/API data.
- this data is sent to `XMsgProcessingUtil` class to process further checks  and validations.
- `XMsgProcessingUtil` class use `GupShupWhatsappAdapter` to convert Webhook/API data into XMessage format.