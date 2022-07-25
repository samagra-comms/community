# Create an Adapter

## 1. Overview

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vise versa.

## 2. How to setup adapter

Setup the adapter repository to start working on it. Follow the steps given to add a new adapter & test it.

1.  Fork the below repository & clone it.

    `https://github.com/samagra-comms/adapter/`
2. Checkout to the recent branch. Eg. release-4.9.0
3. Open the project in any IDE spring/intellij.
4. Follow [point 3](create-an-adapter.md#3-creating-your-own-adapters) to create a new adapter.
5. Write test cases for inbound method `convertMessageToXMsg` & outbound method `processOutBoundMessageF`.
6. A simple example to test the send text message is given in the [file](https://github.com/samagra-comms/adapter/blob/release-4.8.0/src/test/java/com/uci/adapter/netcore/whatsapp/NetcoreServiceTest.java).

## 3. Creating your own Adapters

All adapters are named as `<ProviderName><ChannelName>Adapter`; for example GupshupWhatsappAdapter. Adapters should extend `AbstractProvider` and implement `IProvider`. Thus, it needs to implement the following methods:

* `public Mono<XMessage> convertMessageToXMsg(Object msg) // Converts API response object to XMessage`
* `public Mono<XMessage> processOutBoundMessageF(XMessage nextMsg) //Converts XMessage object to API response and call it.`

These methods are called by `inbound` and `outbound` services internally to process the incoming and outgoing messages.

All adapters with the above implementation will be valid. An example adapter can be found [here](https://github.com/samagra-comms/adapter/blob/release-4.8.0/src/main/java/com/uci/adapter/netcore/whatsapp/NetcoreWhatsappAdapter.java).

### 3.1. Incoming content

Currently we accepts text/media/location/quickReplyButton/list messages. It should be implemeted according to the network provider(Netcore/Gupshup) [documentation](https://wadocs.pepipost.com/webhooks/overview/incoming-message).

To enable these, we have to add them in `convertMessageToXMsg` and convert these according to the [xMessage spec](../uci-basics/)

### 3.2. Outgoing Content

We can send a text message to user. This is the basic usage of an adapter. When creating a new adapter we should start with this scenario. A sample code for the same is also added [here](create-an-adapter.md#4.-sample-code).

We can also send media/location/quickReplyButton/list messages to user. As we are using the ODK forms, we use bind::stylingTags, bind::caption to show the media file or to show the select choices as list/quick reply buttons. There are a few constraints which will be applied with the quickReplyButton/list content.&#x20;

Below are the few styling tags we have allowed for now.

* image //To send image file
* audio //To send audio file
* video //To send video file
* document //To send document file
* buttonsForListItems //To send choices as quick reply buttons
* list //To send choices as list

To send this type of content to user, adapter should implement it according to the network provider(Netcore/Gupshup) [documentation](https://wadocs.pepipost.com/webhooks/overview/incoming-message).

## 4. Adapter Credentials

To make a bot use this adapter we will have to later add this adapter configuration in databse using the [create adapter](../api-documentation/bot-setup-apis.md#2.1-create-adapter) API. This adapter may require you to have some credentials, which can be stored in a vault service. [Click here](../api-documentation/vault-apis.md) to see how to add a secret in vault.&#x20;

## 5. Sample Code

Sample code to receive a text message from channel & reply to channel.

1.  Create a new provider class which implements IProvider.

    * Implement **convertMessageToXMsg** method for converting incoming message from channel to XMessage format.
    * Implement **processOutBoundMessageF** method to reply to channel by converting XMessage format to channel message format.

    ```java
    public class ProviderChannelAdapter extends AbstractProvider implements IProvider {
        @Override
        public Mono<XMessage> convertMessageToXMsg(Object message) throws JAXBException, JsonProcessingException {
            WebMessage webMessage = (WebMessage) message;
            SenderReceiverInfo from = SenderReceiverInfo.builder().deviceType(DeviceType.PHONE).build();
            SenderReceiverInfo to = SenderReceiverInfo.builder().userID("admin").build();
            XMessage.MessageState messageState = XMessage.MessageState.REPLIED;
            MessageId messageIdentifier = MessageId.builder().build();

            XMessagePayload xmsgPayload = XMessagePayload.builder().build();
            xmsgPayload.setText(webMessage.getText());
            XMessage.MessageType messageType= XMessage.MessageType.TEXT;
            from.setUserID(webMessage.getFrom());

            /* To use later in outbound reply message's message id & to */
            messageIdentifier.setChannelMessageId(webMessage.getMessageId());
            messageIdentifier.setReplyId(webMessage.getFrom());

            XMessage x = XMessage.builder()
                    .to(to)
                    .from(from)
                    .channelURI("web")
                    .providerURI("provider")
                    .messageState(messageState)
                    .messageId(messageIdentifier)
                    .messageType(messageType)
                    .timestamp(Timestamp.valueOf(LocalDateTime.now()).getTime())
                    .payload(xmsgPayload).build();

            return Mono.just(x);
        }

        @Override
        public Mono<XMessage> processOutBoundMessageF(XMessage xMsg) throws Exception {
            String phoneNo = "91" +xMsg.getTo().getUserID();
            
            OutboundMessage message = OutboundMessage.builder()
                    .message(Message.builder()
                        .title(xMsg.getPayload().getText())
                        .choices(xMsg.getPayload().getButtonChoices())
                        .msg_type("TEXT")
                        .build())
                    .to(phoneNo)
                    .messageId(xMsg.getMessageId().getChannelMessageId())
                    .build();
            String url = "http://example.com/replyMessageToChannel";

            return WebService.getInstance().
                    sendOutboundMessage(url, outboundMessage)
                    .map(new Function<WebResponse, XMessage>() {
                @Override
                public XMessage apply(WebResponse webResponse) {
                    if(webResponse != null){
                        xMsg.setMessageId(MessageId.builder().channelMessageId(webResponse.getId()).build());
                        xMsg.setMessageState(XMessage.MessageState.SENT);
                    }
                    return xMsg;
                }
            });
            
        }
    }
    ```
2.  Create a web service class that handles api which is used to reply to channel.

    ```java
    @Service
    public class WebService {
        private final WebClient webClient;

        private static WebService webService = null;
        
        public WebService(){
            this.webClient = WebClient.builder().build();
        }

        public static WebService getInstance() {
            if (webService == null) {
                return new WebService();
            } else {
                return webService;
            }
        }

        public Mono<PwaWebResponse> sendOutboundMessage(String url, OutboundMessage outboundMessage) {
            return webClient.post()
                    .uri(url)
                    .body(Mono.just(outboundMessage), OutboundMessage.class)
                    .retrieve()
                    .bodyToMono(WebResponse.class)
                    .map(new Function<WebResponse, WebResponse>() {
                        @Override
                        public WebResponse apply(WebResponse webResponse) {
                            if (webResponse != null) {
                                System.out.println("MESSAGE RESPONSE " + webResponse.getMessage());
                                System.out.println("STATUS RESPONSE " + webResponse.getStatus());
                                System.out.println("MESSAGE ID RESPONSE " + webResponse.getId());
                                return webResponse;
                            } else {
                                return null;
                            }
                        }
                    }).doOnError(new Consumer<Throwable>() {
                        @Override
                        public void accept(Throwable throwable) {
                            System.out.println("ERROR IS " + throwable.getLocalizedMessage());
                        }
                    });
        }
    }
    ```
3.  Create a class for format which is accepted by reply to channel api Eg. **http://example.com/replyMessageToChannel**.

    ```java
    @Getter
    @Setter
    @Builder
    class OutboundMessage {
        private Message message;
        private String to;
        private String messageId;
    }
    ```
4.  Create a message format class for OutboundMessage class property.

    ```java
    @Getter
    @Setter
    @Builder
    class Message {
        String title;
        private ArrayList<ButtonChoice> choices;
        private String msg_type;
    }
    ```
5.  Create a class that accepts response from channel reply api.

    ```java
    @Getter
    @Setter
    class WebResponse {
        private String id;
        private String status;
        private String message;
    }
    ```
6.  Create a class that accepts format for message received from channel.

    ```java
    @Getter
    @Setter
    class WebMessage extends CommonMessage {
        String messageId;

        String text;

        String from;
    }
    ```
7. If we want to fetch the credentials of the adapter from vault service, see the code of [FirebaseNotificationAdapter](https://github.com/samagra-comms/adapter/blob/release-4.9.0/src/main/java/com/uci/adapter/firebase/web/FirebaseNotificationAdapter.java) to check how to do this. &#x20;

## 6. Adapter implementation

After the adapter is created we should be able to implement it in inbound & outbound service for receiving and sending messages to user. [Click here](adapter-implementation.md) to see the process & sample code on how to to do this.

## 7. List of Existing Adapter Implementations

* [Gupshup-Whatsapp](https://github.com/samagra-comms/adapter/blob/release-4.9.0/src/main/java/com/uci/adapter/gs/whatsapp/GupShupWhatsappAdapter.java)
* [Netcore-Whatsapp](https://github.com/samagra-comms/adapter/blob/release-4.9.0/src/main/java/com/uci/adapter/netcore/whatsapp/NetcoreWhatsappAdapter.java)
* [UCI Web Channel](https://github.com/samagra-comms/adapter/blob/release-4.9.0/src/main/java/com/uci/adapter/pwa/PwaWebPortalAdapter.java)
* [Firebase Adapter](https://github.com/samagra-comms/adapter/blob/release-4.9.0/src/main/java/com/uci/adapter/firebase/web/FirebaseNotificationAdapter.java)
