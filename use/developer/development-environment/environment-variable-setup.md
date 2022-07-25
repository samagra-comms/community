# Environment variable setup

### 1. Add enviorment variable in IDE

#### 1.1 Kafka

Kafka is used to build real-time streaming data pipelines and real-time streaming applications. A data pipeline reliably processes and moves data from one system to another, and a streaming application is an application that consumes streams of data.

We use XMessage to converse between our services, It is sent to Kafka topics. Then different services consume these XMessages from Kafka and process further.

* Setup Kafka with [quickstart](https://kafka.apache.org/quickstart) guide.
* Generate the following Kafka topics and add following values :

```
    BOOTSTRAP_SERVERS = # Kafka bootstrap server host/IP
    REGISTRY_URL = # Schema Registry URL generated from kafka
    # kafka topics for sending and receiving XMessage from our services
    KAFKA_CAMPAIGN_TOPIC=campaign
    KAFKA_INBOUND_ERROR_TOPIC=inbound-error
    KAFKA_INBOUND_GS_OPTED_OUT_TOPIC=gs-opted-out
    KAFKA_INBOUND_PROCESSED_TOPIC=inbound-processed
    KAFKA_INBOUND_UNPROCESSED_TOPIC=inbound-unprocessed
    KAFKA_LOGS_TOPIC=uci-telemetry-logs
    KAFKA_ODK_TRANSFORMER_TOPIC=com.odk.transformer
    KAFKA_ODK_TRANSFORMER_TOPIC_PATTERN=com.odk.*
    KAFKA_OUTBOUND_TOPIC=outbound
    KAFKA_TELEMETRY_TOPIC=telemetry
    KAFKA_MESSAGE_REPORT_TOPIC=message-report
    KAFKA_PROCESS_OUTBOUND=process-outbound
    KAFKA_BROADCAST_TRANSFORMER_TOPIC=broadcast-transformer
    KAFKA_GENERIC_TRANSFORMER_TOPIC=generic-transformer
```

#### 1.2 Redis :

We are using Redis for caching the latest XMessage sent/received by user. We would be further using this for caching other elements.

* Host Redis Using Redis [Quickstart](https://redis.io/topics/quickstart) and add below values :

```
    REDIS_DB_INDEX = # Database Index
    REDIS_HOST = # Redis Host
    REDIS_PORT = # Redis port
```

#### 1.3 ODK (Open Data Kit):

ODK is being used to define the flow/logic of a bot. use this link [ODK](https://docs.getodk.org/aggregate-digital-ocean/) to set up and generate its credentials.

```
    ODK_PASS = # ODK Passowrd
    ODK_URL = # ODK URL
    ODK_USER = # ODK Username
```

#### 1.4 FusionAuth :

Fusion Auth is being used to store and Authenticate user data it also stores the user consent to fetch its data (in decrypted form). Deploy Fusion Auth with installation [Guide](https://fusionauth.io/docs/v1/tech/installation-guide/docker).

* Generate Fusion Auth Key and Url using [Documentation](https://fusionauth.io/docs/v1/tech/apis/keys#generate-a-key).

```
    FUSIONAUTH_KEY = # Generated Fusion Key
    FUSIONAUTH_URL = # Generated Fusion URL
```

#### 1.5 Assessment answer comparison characters :

We are using assessment characters to go to the previous state or restart the conversation.

```
    # To restart the bot
    ASSESSMENT_GO_TO_START_CHAR=*
    # To go back to the previous state
    ASSESSMENT_ONE_LEVEL_UP_CHAR=#
```

#### 1.6 Caffeine :

Caffeine is a high-performance Java 8 based caching library providing a near-optimal hit rate. It provides an in-memory cache very similar to the Google Guava API.

Caffeine is being used to cache all the API calls to [federation service](https://github.com/samagra-comms/uci-apis/).

```
    CAFFEINE_CACHE_EXPIRE_DURATION=300
    CAFFEINE_CACHE_MAX_SIZE=0
```

#### 1.7 Postgresql :

PostgreSQL is an advanced, enterprise-class, and open-source relational database system. PostgreSQL supports both SQL (relational) and JSON (non-relational) querying.

It is being used to store the question and assessment data and respective to ODK form questions.

* Setup postgreSQL using [guide](https://zhao-li.medium.com/getting-started-with-postgresql-using-docker-compose-34d6b808c47c).
* Create a new Database and add the following values :

```
    FORMS_DB_HOST = # Database Host
    FORMS_DB_NAME = # Database Name
    FORMS_DB_USERNAME = # Database Username
    FORMS_DB_PASSWORD = # Database Password
    FORMS_DB_PORT = # Port
    FORMS_DB_URL = # Database URL
```

#### 1.8 Netcore :

Netcore is a service provider which is being used to sent/receive messages from the user for various channels (WhatsApp, Telegram, etc.)

Please contact the Netcore administrator to get these details

```
    NETCORE_WHATSAPP_AUTH_TOKEN = # Authentication Token 
    NETCORE_WHATSAPP_SOURCE = # Source ID for sending messages to Netcore 
    NETCORE_WHATSAPP_URI = # Netcore API Base URL
```

#### 1.9 Cassandra :

Cassandra is one of the most efficient and widely-used NoSQL databases. One of the key benefits of this system is that it offers highly-available service and no single point of failure.

Cassandra is being used to store XMessages.

deploy Apache [Cassandra](https://hub.docker.com/r/bitnami/cassandra/) and add below values

```
    CASSANDRA_KEYSPACE = # Keyspace name
    CASSANDRA_PORT = # Cassandra Port
    CASSANDRA_URL = # Cassandra URL
    CASSANDRA_MIGRATION_COUNT = 1 #Count of cassandra queries already exceuted
```

#### 1.10 Campaign URL and token :

To connect to [federation service](https://github.com/samagra-comms/uci-apis/) API use the below details:

Generate campaign admin token using [uci-apis](https://github.com/samagra-comms/uci-apis) project.

```
    CAMPAIGN_ADMIN_TOKEN = # Authentication token
    CAMPAIGN_URL = # federation service base URl
```

#### 1.12 Transport Socket Base URL :

Outbound base URL for Diksha service provider. We can generate Transport Socket Base URL with [transport-socket](https://github.com/samagra-comms/transport-socket/tree/uci-pwa) Project

```
    # Sunbird Adapater Outbound URL
    TRANSPORT_SOCKET_BASE_URL = # Transport Socket Base URL
```

#### 1.13 Exhaust Telemetry

We are sending events to telemetry for exhaust generation, the telemetry specs being used is [here](https://github.com/sunbird-specs/Telemetry/blob/master/learn/specification.md).\
the PData is a unique id assigned to this component

```
    TELEMETRY_EVENT_PDATA_ID = # Telemetry Exhaust PDATA
    EXHAUST_TELEMETRY_ENABLED=TRUE
```

#### 1.14 Posthog

We can send telemetry to posthog account instead of exhaust telemetry. To enable this add below environment variables.

```
    POSTHOG_TELEMETRY_APIKEY= // Api key
    POSTHOG_TELEMETRY_URL=https://app.posthog.com
    POSTHOG_EVENT_ENABLED=TRUE
```

**1.15 Azure**&#x20;

Azure storage is a service added for media upload & display functionality. We can choose to use Azure or minio for this. To use this add below env variables.&#x20;

```
    AZURE_BLOB_STORE_ACCOUNT_KEY=accountName
    AZURE_BLOB_STORE_ACCOUNT_NAME=accountKey
    AZURE_BLOB_STORE_CONTAINER=container
    SELECTED_FILE_CDN=azure
```

&#x20;&#x20;

**1.16 Minio**

Minio storage is another service for media upload & display functionality. We can choose to use Azure or minio for this. To use this add below env variables.

```
    CDN_MINIO_LOGIN_ID=loginID
    CDN_MINIO_URL=url
    CDN_MINIO_PASS=pass
    CDN_MINIO_FA_URL=fa_url
    CDN_MINIO_FA_KEY=fa_key
    CDN_MINIO_BUCKET_ID=bucketID
    CDN_MINIO_APP_ID=appID
    SELECTED_FILE_CDN=minio
```

**1.17 Vault**

Vault is a service which is being used to store credentials related to various adapters. For example firebase service key, gupshup api credentials etc. We need a token to access this service, Please [contact the administrator](../../contact-the-administrator.md) for this token.

```
VAULT_FUSION_AUTH_URL=fa_url
VAULT_FUSION_AUTH_TOKEN=fa_key
VAULT_SERVICE_URL=http://143.110.183.73:3001
VAULT_SERVICE_TOKEN=token
```

**1.18 Other environment variables**

```
    # In which enviorment we are working
    ENV = dev 
    WEBCLIENT_HTTP_REQUEST_TIMEOUT = 5
```
