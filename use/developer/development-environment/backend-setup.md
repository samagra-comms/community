# Backend Setup

### Getting Started

### 1. Introduction

The Unified Communications Interface (UCI) aims to democratize the use of different communication channels such as WhatsApp, Telegram, SMS, email and more for governance use cases through a standard configurable manner that is reusable and scalable across all governance use cases.

### 2. Overview

This Document help you to Setup UCI (Unified Communications Interface) Project and test APIs on your local machine.

### 3. Prerequisite

1. Install Git

```
    $ sudo apt update
    $ sudo apt install git
```

1. Install Java 11

```
    $ sudo apt-get install openjdk-11-jdk
```

1. Install Docker
   * install docker using [docker](https://docs.docker.com/engine/install/ubuntu/) installation guide.
2. Install Cassandra
   * install cassandra using [cassandra](https://cassandra.apache.org/doc/latest/cassandra/getting\_started/installing.html) installation guide.
3. Install Maven

```
    $ sudo apt install maven
```

1. Lombok Enabled
   * Enable [lombok](https://www.baeldung.com/lombok-ide) in eclipse / STS / intelliJ.
2. Install IDE ([STS](https://spring.io/tools) / [Eclipse](https://www.eclipse.org/downloads/) / [IntelliJ](https://www.jetbrains.com/idea/download/))
3. Install [Postman](https://www.postman.com/downloads/) (for testing API)
4. Install Kafka, Zookeeper
   * install kafka using [kafka](https://www.onlinetutorialspoint.com/kafka/how-to-install-apache-kafka-on-ubuntu-18-04.html) installation guide.
5. Install Redis
   * Install Redis using installation [guide](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04).
6. Install Postgresql
   * Setup PostgreSQL using [Quickstart](https://www.postgresql.org/download/linux/ubuntu/) guide.

### 4. Setup

#### 4.1 For first time :

1. Fork following repositories :

```
    https://github.com/samagra-comms/dao
    https://github.com/samagra-comms/utils
    https://github.com/samagra-comms/message-rosa
    https://github.com/samagra-comms/adapter
    https://github.com/samagra-comms/outbound
    https://github.com/samagra-comms/orchestrator
    https://github.com/samagra-comms/inbound
    https://github.com/samagra-comms/transformer
```

1. Clone all forked repositories

```
    git clone repository-link
```

1. Import all cloned repos into IDE
2. if project is not build by default then build using :

```
    $ mvn clean install -U 
```

1. Add [Enviorment Variable](../../docs/environment-variables.md) in IDE.

#### 4.2 Routine :

1. Run spring boot application for following projects :
   * Inbound
   * Orchestrator
   * Transformer
   * Outbound

### 5. Testing API

#### 5.1 For first time :

1. import following files to postman :

```
    Samagra Inbound - Apis.postman_collection
    Samagra Transformer - Apis.postman_collection
```

1.  create new enviorment **inbound** with properties :

    | Variable |   Type  |  Initial Value |  Current Value |
    | :------: | :-----: | :------------: | :------------: |
    |  baseUrl | default | localhost:8085 | localhost:8085 |
2.  create new enviorment **transformer** with following properties :

    | Variable |   Type  |  Initial Value |  Current Value |
    | :------: | :-----: | :------------: | :------------: |
    |  baseUrl | default | localhost:9091 | localhost:9091 |

#### 5.2 Routine :

1. Test bot APIs with :

```
    Collections
    .
    └── Samagra Inbound - Apis 
                └── Bot - Messages
                        └── ...
```