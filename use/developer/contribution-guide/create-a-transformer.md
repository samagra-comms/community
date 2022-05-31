# Create a Transformer

## 1. Overview

Transformers transforms the previous xMessage from the user to one that needs to be sent next. It is essentially a microservice that based on the previous user action, returns a new xMessage that will then be shown to the user. This also enables conversion from one type

All inbound messages pass through a transformer. If a transformer is not assigned, then a null transformer is assigned to the xMessage. Since the current implementation of MessageRosa is only in Java, currently there is a limitation on the number of languages you can build the transformer on which right now is just Java.

Simply put a transformer looks something like this ![](https://samagra-development.github.io/docs/img/transformer.jpg)

### 1.1 Responsibilities

- Acts as a state machine and converts messages from one state to another.
- Used to transform messages
- Applies conversational logic (constraints or input format)

## 2. Creating your own Transformer

All transformers are named as `<Action>Transformer`; for example PDFTransformer will generate a PDF from the previous XMessage and convert it to an XMessage. Adapters should extend `TransformerProvider`. Thus, it needs to implement the following methods:

- ```java
    public XMessage transform(XMessage nextMsg) //Takes in XMessage and return the next message.
  ```

## 3. Sample Code
Sample code to transform text to lowercase characters before sendind to outbound

    ```java
        public class DemoTransformer extends TransformerProvider {
            private final Flux<ReceiverRecord<String, String>> reactiveKafkaReceiver;
            
            @Value("${processOutbound}")
            private String processOutboundTopic;

            @EventListener(ApplicationStartedEvent.class)
            public void onMessage() {
                reactiveKafkaReceiver
                        .doOnNext(new Consumer<ReceiverRecord<String, String>>() {
                            @Override
                            public void accept(ReceiverRecord<String, String> stringMessage) {
                                try {
                                    XMessage msg = XMessageParser.parse(new ByteArrayInputStream(stringMessage.value().getBytes()));
                                    transform(msg)
                                            .subscribe(new Consumer<XMessage>() {
                                                @Override
                                                public void accept(XMessage transformedMessage) {
                                                    if (transformedMessage != null) {
                                                        try {
                                                            kafkaProducer.send(processOutboundTopic, transformedMessage.toXML());
                                                        } catch (JAXBException e) {
                                                            e.printStackTrace();
                                                        }
                                                    }
                                                }
                                            });
                                } catch (JAXBException e) {
                                    e.printStackTrace();
                                } catch (NullPointerException e) {
                                    log.error("An error occured : "+e.getMessage() + " at line no : "+ e.getStackTrace()[0].getLineNumber()
                                            +" in class : "+e.getStackTrace()[0].getClassName());
                                } catch (Exception e) {
                                    e.printStackTrace();
                                }
                            }
                        })
                        .doOnError(new Consumer<Throwable>() {
                            @Override
                            public void accept(Throwable e) {
                                System.out.println(e.getMessage());
                                log.error("KafkaFlux exception", e);
                            }
                        }).subscribe();

            }

            /* Transform text to lower case */
            public XMessage transform(XMessage nextMsg) {
                Payload payload = nextMsg.getPayload();
                String text = payload.getText();
                payload.setText(text.toLowerCase());
                nextMsg.setPayload(payload);
                return nextMsg;
            }
        }
    ```


All transformers with the implementation same as above exmaple will be valid. A detailed example transformer can be found [here](https://github.com/samagra-comms/transformer/blob/release-4.9.0/src/main/java/com/uci/transformer/odk/ODKConsumerReactive.java).

## 4. Properties of a transformer

- Transformers could call external services and wait for resposes from them to process message. These transformers are called `call-only` transformer. An example of this transformer is the PDF service transformer which calls the PDF service to check the status of queue of previous message and responds with a response containing PDF for the user.
- All transformers need to be registered. All unregistered transformers will not be acted upon. This is what essentially adds a topic to broker on which the messages are pushed. Also this requires some basic config of the max time a transformer could take to process the message.
- Scaling of transformers is done horizontally but the broker needs to know the number so tha partitions can be reconfigured.
- Since it's part of a state machine. If the transformer is stuck it needs communicate to the Orchestrator to that it can be escalate.
