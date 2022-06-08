# Adapter

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to XMessages and vice versa. Adapters are the gateway to the external services and are responsible for receiving user response and sending the response to users.

## 1. Netcore Whatsapp Adapter

`NetcoreWhatsappAdapter` class is used to convert API/Webhook data received from Netcore Service into XMessage Formate.\
this class uses the following methods for conversion:

- **convertMessageToXMsg**: Used in Inbound to convert API/Webhook data provided by Netcore into XMessage.

- **processOutBoundMessageF**: Used in Outbound to convert XMessage back to Netcore API/Webhook data.

## 2. GupShup Whatsapp Adapter

`GupShupWhatsappAdapter` class is used to convert API/Webhook data received from GupShup Service into XMessage Formate.\
this class uses the following methods for conversion:

- **convertMessageToXMsg**: Used in Inbound to convert API/Webhook data provided by GupShup into XMessage.

- **processOutBoundMessageF**: Used in Outbound to convert XMessage back to GupShup API/Webhook data.

## 3. Sunbird Web Portal Adapter

`SunbirdWebPortalAdapter` class is used to convert API/Webhook data received from Netcore Service into XMessage Formate.\
this class uses the following methods for conversion:

- **convertMessageToXMsg**: Used in Inbound to convert API/Webhook data provided by Sunbird Web Service into XMessage.

- **processOutBoundMessageF**: Used in Outbound to convert XMessage back to Sunbird API/Webhook data.

