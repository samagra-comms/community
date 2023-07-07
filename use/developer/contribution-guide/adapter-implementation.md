# Adapter Implementation

## 1. Overview

A new adapter can be created using this [link](create-an-adapter.md). Once the adapter is created we can incorporate this apater in inbound and outbound service for receiving and sending message to user respectively.

Below is the process on how to implement a adapter in inbound & outbound.&#x20;

## 2. Inbound Implementation

Inbound receives the messages from a channel, and uses the channel adapter to convert it to XMessage format. Once the messages is converted it will get processed.&#x20;

When a new adapter is created we will have to add a class that will receive the messages from channel and use the adapter to convert this message to XMessage.&#x20;

In the next section of the document, we will see the sample code for the same.

### 2.1 Setup inbound

Follow the steps below to setup the inbound repository to start working on it.&#x20;

1.  Fork the below repository & clone it.

    `https://github.com/samagra-comms/inbound/`
2. Checkout to the recent stable `master` branch.
3. Open the project in any IDE spring/intellij.
4. Follow the next steps.

### 2.2 Sample Code

Create a class for the channel & provider for the adapter created. **Eg**. NetcoreWhatsappController.&#x20;

```
@Slf4j
@RestController
@RequestMapping(value = "/provider")
public class ProviderChannelController {

    @Value("${inboundProcessed}")
    private String inboundProcessed;

    @Value("${inbound-error}")
    private String inboundError;

    private ProviderChannelAdapter providerChannelAdapter;

    @Autowired
    public SimpleProducer kafkaProducer;

    @Autowired
    public XMessageRepository xmsgRepo;

    @Autowired
    public BotService botService;

    @Autowired
    public RedisCacheService redisCacheService;

    @Value("${outbound}")
    public String outboundTopic;

    @Value("${messageReport}")
    public String topicReport;

    @RequestMapping(value = "/channel", method = RequestMethod.POST, consumes = MediaType.APPLICATION_JSON_VALUE)
    public void providerChannel(@RequestBody WebMessage message) throws JsonProcessingException, JAXBException {

        providerChannelAdapter = ProviderChannelAdapter.builder().build();

        XMsgProcessingUtil.builder()
                .adapter(providerChannelAdapter)
                .xMsgRepo(xmsgRepo)
                .inboundMessage(message)
                .topicFailure(inboundError)
                .topicSuccess(inboundProcessed)
                .kafkaProducer(kafkaProducer)
                .botService(botService)
                .redisCacheService(redisCacheService)
                .topicOutbound(outboundTopic)
                .topicReport(topicReport)
                .build()
                .process();
    }
}
```

Once the class is create, we can use the link **http://host:port/provider/channel** to receive messages from user. A user will receive a message in below format.

```
{
    "messageId": "67630c26-e29e-46ae-acb6-756704362d84",
    "text": "Hi",
    "from": "98********"
}
```

A sample api to receive messages from user is given below.

```
curl --location --request POST 'localhost:8085/provider/channel' \
--header 'Content-Type: application/json' \
--data-raw '{
    "messageId": "67630c26-e29e-46ae-acb6-756704362d84",
    "text": "Hi",
    "from": "98********"
}'
```

### 2.3 Existing Implementations

Below are some of the existing implementation code of currently available adapters.

* [Netcore Whatspp](https://github.com/samagra-comms/inbound/blob/release-4.9.0/src/main/java/com/uci/inbound/netcore/NetcoreWhatsappConverter.java)
* [Gupshup Whatsapp](https://github.com/samagra-comms/inbound/blob/release-4.9.0/src/main/java/com/uci/inbound/incoming/GupShupWhatsappConverter.java)
* [UCI Web Channel](https://github.com/samagra-comms/inbound/blob/release-4.9.0/src/main/java/com/uci/inbound/pwa/web/PwaWebController.java)

## 3. Outbound Implementation

Outbound converts the xMessage to the one that will be sent to the channel(sms/whatsapp). It will then be sent to the network provider who will send it to the channel using the implementation done in adapter.

Outbound uses this [OutboundKafkaController](https://github.com/samagra-comms/outbound/blob/release-4.9.0/src/main/java/com/uci/outbound/consumers/OutboundKafkaController.java) class to send messages to user. There is class called [ProviderFactory](https://github.com/samagra-comms/adapter/blob/release-4.9.0/src/main/java/com/uci/adapter/provider/factory/ProviderFactory.java) which uses the channel & provider set in the XMessage and return the adapter object which will be used to call the **processOutBoundMessageF** method.&#x20;

### 3.1 Sample code

Add code to return adapter object based on the channel & provider set in XMessage.

```
if(provider.toLowerCase().equals("specifiedProvider") && channel.toLowerCase().equals("specifiedChannel")){
    return ProviderChannelAdapter.builder().build();
}
```

Once the code is added, we should be able to use the new adapter to sent messages to user.
