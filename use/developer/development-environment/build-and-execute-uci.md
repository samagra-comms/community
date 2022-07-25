# Build and Execute UCI

## 1. Overview

UCI has a total of 9 repositories which are being used to process the bot messages from inbound to outbound. We have 4 main repositories who will process the messages from receiving the messages from channel(Sms/Whatsapp), converting the message to xmessage, authenticating the user, producing the next message, converting it back to message format for channel and sending it to the channel(Sms/Whatsapp). The other repositories are the helper repositories with each doing different jobs.

## 2. Build Flow

The repositories should be built & then executed in the sequence mentioned below.

### **2.1. Utils**

Utils communicate with the federation service via apis. It holds all the utility/helper classes which will be used in other repositories. To clone this git repository use [link](https://github.com/samagra-comms/utils). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

### **2.2. Message Rosa**

Message Rosa holds all the core models for XMessage & its property fields. To clone this git repository use [link](https://github.com/samagra-comms/message-rosa). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

### **2.3. Dao**

Dao holds the dao & repository classes for XMessage. To clone this git repository use [link](https://github.com/samagra-comms/dao). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

### **2.4. Adapter**

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vice versa. Adapters are gateway to the external services and are responsible for receiving user response and sending the response to users. To clone this git repository use [link](https://github.com/samagra-comms/adapter). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

### **2.5. Inbound**

Inbound receives the messages from a channel, and uses the channel adapter to convert it to XMessage format. Once the message is converted, it will be pushed to the kafka topic, the orchestrator will listen to this topic to further process it. To clone this git repository use [link](https://github.com/samagra-comms/inbound). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

We can use any IDE tool to run it on a local machine or we can also run this service on terminal using below command.

```
    mvn spring-boot:run
```

We can also create its docker image & run the service on any server/local machine.

Create docker image

```
    docker build -t samagragovernance/inbound:${CURRENT_VERSION} .
```

Push docker image to docker hub

```
    docker push samagragovernance/inbound:${CURRENT_VERSION}
```

Run docker image

```
    docker-compose -f docker-compose.yml up -d inbound
```

### **2.6. Orchestrator**

Orchestrator authenticates & processes the user data from whom the message is received. The XMessage will then be pushed to the transformers topic based on the conversation configuration, the corresponding transformer(transformer/broadcast-transformer) will listen to this topic to further process it. To clone this git repository use [link](https://github.com/samagra-comms/orchestrator). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

We can use any IDE tool to run it on a local machine or we can also run this service on terminal using below command.

```
    mvn spring-boot:run
```

We can also create its docker image & run the service on any server/local machine.

Create docker image

```
    docker build -t samagragovernance/orchestrator:${CURRENT_VERSION} .
```

Push docker image to docker hub

```
    docker push samagragovernance/orchestrator:${CURRENT_VERSION}
```

Run docker image

```
    docker-compose -f docker-compose.yml up -d orchestrator
```

### **2.7. Transformer**

This is a ODK transformer, which transforms the previous xMessage from the user to one that needs to be sent next. It is a microservice that returns a new xMessage based on the previous user action, which will then be shown to the user. The XMessage will be pushed to the process-outbound topic, orchestrator listens to this topic and will further process it to & send it to outbound topic. To clone this git repository use [link](https://github.com/samagra-comms/transformer). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

We can use any IDE tool to run it on a local machine or we can also run this service on terminal using below command.

```
    mvn spring-boot:run
```

We can also create its docker image & run the service on any server/local machine.

Create docker image

```
    docker build -t samagragovernance/transformer:${CURRENT_VERSION} .
```

Push docker image to docker hub

```
    docker push samagragovernance/transformer:${CURRENT_VERSION}
```

Run docker image

```
    docker-compose -f docker-compose.yml up -d transformer
```

### **2.8. Broadcast Transformer**

Broadcast transformer transforms the single message to multiple messages for each user segment associated with a conversation. It is a micro service that generated multiple XMessage and push it to process-outbound topic, orchestrator listens to this topic and will further process it to & send it to outbound topic. To clone this git repository use [link](https://github.com/samagra-comms/broadcast-transformer). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

We can use any IDE tool to run it on a local machine or we can also run this service on terminal using below command.

```
    mvn spring-boot:run
```

We can also create its docker image & run the service on any server/local machine.

Create docker image

```
    docker build -t samagragovernance/broadcast-transformer:${CURRENT_VERSION} .
```

Push docker image to docker hub

```
    docker push samagragovernance/broadcast-transformer:${CURRENT_VERSION}
```

Run docker image

```
    docker-compose -f docker-compose.yml up -d broadcast-transformer
```

### **2.9. Outbound**

Outbound converts the xMessage to the one that will be sent to the channel(sms/whatsapp). It will then be sent to the network provider(Netcore/Gupshup) who will send it to the channel. To clone this git repository use [link](https://github.com/samagra-comms/outbound). Once the repository is cloned, build it using the command below.

```
    mvn clean install -DskipTests
```

We can use any IDE tool to run it on a local machine or we can also run this service on terminal using below command.

```
    mvn spring-boot:run
```

We can also create its docker image & run the service on any server/local machine.

Create docker image

```
    docker build -t samagragovernance/outbound:${CURRENT_VERSION} .
```

Push docker image to docker hub

```
    docker push samagragovernance/outbound:${CURRENT_VERSION}
```

Run docker image

```
    docker-compose -f docker-compose.yml up -d outbound
```
