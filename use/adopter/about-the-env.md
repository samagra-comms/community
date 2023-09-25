## Environment Variables Explanation

## 1. `POSTHOG_API_KEY`

The `POSTHOG_API_KEY` environment variable is used for authentication with the PostHog service. PostHog is a product analytics platform that helps you track user interactions with your application. To obtain this API key:

1. Log in to your PostHog dashboard.
2. Navigate to the API settings or account settings section.
3. Generate or retrieve your API key.
4. Set the `POSTHOG_API_KEY` environment variable to the obtained key.

## 2. `FIREBASE_SERVER_KEY`

The `FIREBASE_SERVER_KEY` environment variable is typically used for Firebase Cloud Messaging (FCM) to send push notifications to mobile devices. To obtain this key:

1. Log in to your Firebase Console.
2. Create or select a project.
3. Navigate to the project settings.
4. Under the Cloud Messaging tab, find the "Server Key" or "Legacy Server Key."
5. Set the `FIREBASE_SERVER_KEY` environment variable to this key.

## 3. `NL Depenencies`

NL app is our prime use case for UCI. UCI uses NL services for various tasks, priamrary of which is user authentication. Refer to [this](https://github.com/Mission-Prerna/sandbox-deployment) if you want to setup NL. We have used the [NL Sandbox](https://sandbox.bot.nl.samagra.io) for initial setup purpose. 

### NLAPP_USER_URL
- URL for an NL app dependency that searches for users by query.

### NLAPP_USER_AUTH
- Authentication token for accessing the NL app user search API.

### NLAPP_USER_XAPPID
- X-App-Id for the NL app.

### NG_APP_url
- URL for an NG app.

### NG_APP_nl_url
- URL for the NL component of the NG app.

### NG_APP_user_segment_url
- URL for user segmentation in the NG app.

### NG_APP_nl_login_url
- URL for NL login in the NG app.

### NG_APP_nl_login_token
- Token for NL login in the NG app.

### NG_APP_nl_application_id
- Application ID for the NL app in NG.

### NG_APP_blobUrl
- Blob URL for the NG app.

### NG_APP_botPhoneNumber
- Phone number for the bot in the NG app.

### NG_APP_adapterId
- Adapter ID for the NG app.

### NG_APP_broadcastAdapterId
- Broadcast adapter ID for the NG app.

### NG_APP_userId
- User ID for the NG app.

### NG_APP_orgId
- Organization ID for the NG app.

### NG_APP_token
- Token for the NG app.

## 4. Minio CDN Configuration:

The following environment variables are used to configure Minio CDN, a distributed object storage system.

## `CDN_MINIO_LOGIN_ID`

This variable represents the login ID for accessing Minio CDN.

## `CDN_MINIO_APP_ID`

The `CDN_MINIO_APP_ID` environment variable is typically a unique identifier for your Minio CDN application. It will be provided by minio when you create an application.

## `CDN_MINIO_BUCKET_ID`

This variable specifies the ID or name of the bucket within Minio CDN where your application stores objects. Buckets are used to organize and manage your data within Minio.

## `CDN_MINIO_FA_KEY`

The `CDN_MINIO_FA_KEY` is a token or key used for authentication and authorization with Minio CDN. It may be provided by your CDN service provider.

## `CDN_MINIO_FA_URL`

The `CDN_MINIO_FA_URL` is the URL used for authentication with Minio CDN. This URL is typically provided by your CDN service provider and is used to authenticate and authorize requests to your CDN resources.

## `CDN_MINIO_PASS`

The `CDN_MINIO_PASS` environment variable represents the password or secret associated with your Minio CDN account. This password is used for secure access to your CDN resources.

## `CDN_MINIO_URL`

The `CDN_MINIO_URL` environment variable specifies the base URL for accessing objects stored in Minio CDN. It represents the endpoint for your CDN service.

## 5. Fusionauth Environment Variables:

### FUSIONAUTH_POSTGRES_USER
- Postgres user for Fusionauth service.

### FUSIONAUTH_POSTGRES_DBNAME
- Postgres database name for Fusionauth service.

### FUSIONAUTH_POSTGRES_PASSWORD
- Postgres password for Fusionauth service.

### FUSIONAUTH_POSTGRES_PORT
- Postgres port for Fusionauth service.

### FUSIONAUTH_DATABASE_USERNAME
- Database username for Fusionauth service.

### FUSIONAUTH_DATABASE_PASSWORD
- Database password for Fusionauth service.

### FUSIONAUTH_APP_KICKSTART_FILE
- Kickstart file for Fusionauth application.

### FUSIONAUTH_API_KEY
- API key for Fusionauth.

### ES_JAVA_OPTS
- JVM options for Elasticsearch.

### FUSIONAUTH_MEMORY
- Memory allocation for Fusionauth.

### FUSIONAUTH_APP_MEMORY
- Memory allocation for Fusionauth application.

### FUSIONAUTH_ADMIN_PASSWORD
- Admin password for Fusionauth.

### FUSIONAUTH_ADMIN_EMAIL
- Admin email for Fusionauth.

## 6. ODK Environment Variables:

### ODK_POSTGRES_PASSWORD
- Postgres password for ODK.

### ODK_POSTGRES_USER
- Postgres user for ODK.

### ODK_POSTGRES_DB
- Postgres database for ODK.

## 7. Environment Variables for UCI-APIs:

## Postgresql

### PSQL_DB_URL
- Postgres database URL for UCI APIs.

### UCI_API_POSTGRES_PASSWORD
- Postgres password for UCI APIs.

### UCI_API_POSTGRES_USER
- Postgres user for UCI APIs.

### UCI_API_POSTGRES_DB
- Postgres database for UCI APIs.

### POSTGRES_HOST
- Hostname for PostgreSQL.

### POSTGRES_USER
- PostgreSQL user.

### POSTGRES_PASSWORD
- PostgreSQL password.

### POSTGRES_DB
- PostgreSQL database.

## 8. Kafka

### KAFKA_HOST_DEV
- Hostname for Kafka development environment.

### KAFKA_USER
- Kafka user.

### KAFKA_PASS
- Kafka password.

### KAFKA_HOST
- Hostname for Kafka.

### KAFKA_PORT
- Kafka port.

### KAFKA_LOG_LEVEL
- Kafka log level.

### ODK_SERVICE
- URL for the ODK service.

### ODK_USERNAME
- ODK username.

### ODK_PASSWORD
- ODK password.

### ODK_BASE_URL
- Base URL for ODK.

### FUSIONAUTH_URL
- URL for Fusionauth in UCI.

### FUSIONAUTH_KEY
- Fusionauth key.

### ADMIN_TOKEN
- Admin token.

### FUSIONAUTH_ANONYMOUS_BOT_APP_ID
- Application ID for anonymous bot in Fusionauth.

### FUSIONAUTH_ADMIN_PASS
- Admin password for Fusionauth.

### AUTH_PUBLIC_KEY
- Public key for authentication.

### POSTHOG_BASE_URL
- Base URL for PostHog.

### POSTHOG_BATCH_SIZE
- Batch size for PostHog.

### POSTHOG_FLUSH_INTERVAL
- Flush interval for PostHog.

## 9. Hasura

### HASURA_GRAPHQL_DATABASE_URL
- Database URL for Hasura GraphQL.

### HASURA_GRAPHQL_ENABLE_CONSOLE
- Enable Hasura GraphQL console.

### HASURA_GRAPHQL_ENABLED_LOG_TYPES
- Enabled log types for Hasura GraphQL.

### HASURA_GRAPHQL_ADMIN_SECRET
- Admin secret for Hasura GraphQL.

### UCI_CORE_BASE_URL
- Base URL for UCI Core.

### TRANSFORMER_BASE_URL
- Base URL for the transformer.

### GRAPHQL_BASE_URL
- Base URL for GraphQL in UCI API DB.

### INBOUND_URL
- URL for inbound service.

## 10. Encryption Key

### ENCRYPTION_KEY
- Encryption key.

### ENV
- Environment (e.g., prod).

## 11. UCI Core

### CAMPAIGN_URL
- URL for campaigns in UCI Core.

### CAMPAIGN_ADMIN_TOKEN
- Admin token for campaigns in UCI Core.

## 12. Other Kafka Configurations

### BOOTSTRAP_SERVERS
- Bootstrap servers for Kafka.

### KAFKA_INBOUND_PROCESSED_TOPIC
- Kafka topic for inbound processed messages.

### KAFKA_CAMPAIGN_TOPIC
- Kafka topic for campaigns.

### KAFKA_INBOUND_UNPROCESSED_TOPIC
- Kafka topic for inbound unprocessed messages.

### KAFKA_INBOUND_GS_OPTED_OUT_TOPIC
- Kafka topic for inbound GS opted-out messages.

### KAFKA_INBOUND_ERROR_TOPIC
- Kafka topic for inbound error messages.

### KAFKA_OUTBOUND_TOPIC
- Kafka topic for outbound messages.

### KAFKA_TELEMETRY_TOPIC
- Kafka topic for telemetry.

### KAFKA_ODK_TRANSFORMER_TOPIC
- Kafka topic for ODK transformer.

### KAFKA_ODK_TRANSFORMER_TOPIC_PATTERN
- Kafka topic pattern for ODK transformer.

### KAFKA_LOGS_TOPIC
- Kafka topic for telemetry logs.

### KAFKA_BROADCAST_TRANSFORMER_TOPIC
- Kafka topic for broadcast transformer.

### KAFKA_PROCESS_OUTBOUND
- Kafka topic for processing outbound messages.

### KAFKA_MESSAGE_REPORT_TOPIC
- Kafka topic for message reports.

### KAFKA_GENERIC_TRANSFORMER_TOPIC
- Kafka topic for generic transformer.

### KAFKA_NOTIFICATION_TOPIC
- Kafka topic for notification outbound.

### KAFKA_NOTIFICATION_INBOUND_PROCESSED
- Kafka topic for processed inbound notifications.

## 13. FormsDB Configurations

### FORMS_DB_HOST
- Hostname for FormsDB.

### FORMS_DB_NAME
- Database name for FormsDB.

### FORMS_DB_PASSWORD
- Database password for FormsDB.

### FORMS_DB_PORT
- Database port for FormsDB.

### FORMS_DB_URL
- Database URL for FormsDB.

### FORMS_DB_USERNAME
- Database username for FormsDB.

## 14. ODK Configurations

### ODK_PASS
- ODK password.

### ODK_URL
- URL for ODK.

### ODK_USER
- ODK user.

## 15. Cassandra COnfigurations

### CASSANDRA_URL
- Cassandra URL.

### CASSANDRA_PORT
- Cassandra port.

### CASSANDRA_KEYSPACE
- Cassandra keyspace.

### CASSANDRA_MIGRATION_COUNT
- Cassandra migration count.

### CASSANDRA_PASSWORD
- Cassandra password.

## 16. Ports

### INBOUND_INTERNAL_PORT
- Internal port for inbound service.

### INBOUND_EXTERNAL_PORT
- External port for inbound service.

### OUTBOUND_INTERNAL_PORT
- Internal port for outbound service.

### ORCHESTRATOR_INTERNAL_PORT
- Internal port for orchestrator service.

### TRANSFORMER_INTERNAL_PORT
- Internal port for transformer service.

### BROADCAST_TRANSFORMER_INTERNAL_PORT
- Internal port for broadcast transformer service.

## 17. Assesment Check Characters

### ASSESSMENT_GO_TO_START_CHAR
- Character for going to the start of an assessment.

### ASSESSMENT_ONE_LEVEL_UP_CHAR
- Character for moving one level up in an assessment.

### CURRENT_VERSION
- Current version.

### XMSG_USER_DATA_BEFORE_HOUR
- User data before a certain hour.

## 18. PWA Adapter

### PWA_TRANSPORT_SOCKET_BASE_URL
- Base URL for PWA transport socket.

### TRANSPORT_SOCKET_BASE_URL
- Base URL for transport socket.

## 19. Netcore

### NETCORE_WHATSAPP_AUTH_TOKEN
- Authorization token for Netcore WhatsApp.

### NETCORE_WHATSAPP_SOURCE
- Netcore WhatsApp source.

### NETCORE_WHATSAPP_URI
- Netcore WhatsApp URI.

## 20. Redis

### REDIS_DB_INDEX
- Redis database index.

### REDIS_HOST
- Hostname for Redis.

### REDIS_PORT
- Redis port.

### REDIS_NUMBER_PORT
- Redis number port.

### REDIS_PASS
- Redis password.

### REDIS_ENABLED
- Enable or disable Redis.

### TRANSPORT_SOCKET_CACHE_PORT
- Port for transport socket cache.

### TRANSPORT_SOCKET_CACHE_HOST
- Hostname for transport socket cache.

## 21. CDN Config

### SELECTED_FILE_CDN
- Selected file CDN.

### TEMPLATE_SERVICE_BASE_URL
- Base URL for the template service.

### CDAC_BASE_URL
- Base URL for CDAC.

## 22. Vault Credentials

### VAULT_FUSION_AUTH_URL
- URL for Vault Fusionauth.

### VAULT_FUSION_AUTH_TOKEN
- Token for Vault Fusionauth.

### VAULT_SERVICE_URL
- URL for Vault service.

### VAULT_SERVICE_TOKEN
- Token for Vault service.

## 23. Telemetry Events

### EXHAUST_TELEMETRY_ENABLED
- Enable or disable exhaust telemetry.

### POSTHOG_EVENT_ENABLED
- Enable or disable PostHog events.

### TELEMETRY_EVENT_PDATA_ID
- Telemetry event PData ID.

### POSTHOG_TELEMETRY_APIKEY
- API key for PostHog telemetry.

### POSTHOG_TELEMETRY_URL
- URL for PostHog telemetry.

## 24. Doubtnut config

### DOUBTNUT_BASE_URL
- Base URL for DoubtNut.

### DOUBTNUT_WELCOME_MSG
- Welcome message for DoubtNut.

### DOUBTNUT_WELCOME_VIDEO
- Welcome video URL for DoubtNut.

### DOUBTNUT_AUTH_KEY
- Authentication key for DoubtNut.

## 25. FormsDB Graphql Admin Secret

### FORMSDB_HASURA_ADMIN_SECRET
- Admin secret for FormsDB Hasura GraphQL.

## 26. AKHQ

### AKHQ_USERNAME
- Username for AKHQ.

### AKHQ_PASSWORD
- Password for AKHQ.

## 27. VAULT for UCI

### VAULT_TOKEN
- Token for Vault.

### NOTIFICATION_KEY_ENABLE
- Enable or disable notification key.

## 28. Email Config

### EMAIL_HOST
- Hostname for email.

### EMAIL_USERNAME
- Email username.

### EMAIL_PASSWORD
- Email password.

### EMAIL_PORT
- Email port.

### RECIPIENT_EMAILS
- Recipient emails.

## 29. BROADCAST_NOTIFICATION_CHUNK_SIZE
- Chunk size for broadcast notifications.

## 30. Caffeine cache values

### CAFFEINE_CACHE_MAX_SIZE
- Maximum size for Caffeine cache.

### CAFFEINE_CACHE_EXPIRE_DURATION
- Expiry duration for Caffeine cache.

## 31. MINIO 

### MINIO_MEDIA_UPLOAD_URL
- URL for Minio media upload.

### MINIO_GET_SIGNED_FILE_URL
- URL for Minio get signed file.

## 32. Redis Name for Notification Orchestrator 

### NOTIFICATION_KAFKA_CACHE
- Kafka cache name for notifications.

## 33. JVM Size

### JVM_ARGS_INBOUND
- JVM arguments for inbound service.

### JVM_ARGS_ORCHESTRATOR
- JVM arguments for orchestrator service.

### JVM_ARGS_TRANSFORMER
- JVM arguments for transformer service.

### JVM_ARGS_BROADCAST_TRANSFORMER
- JVM arguments for broadcast transformer service.

### JVM_ARGS_OUTBOUND
- JVM arguments for outbound service.

## 34. BotService Utils

### WEBCLIENT_INTERVAL
- Interval for web client.

### WEBCLIENT_RETRY_MAX_ATTEMPTS
- Maximum retry attempts for web client.

### WEBCLIENT_RETRY_MIN_BACK_OFF
- Minimum back-off time for web client retries.

## 35. Redis Data Timeout(Second)

### REDIS_KEY_TIMEOUT
- Redis key timeout in seconds.

## 36. Outbound Notification and Cassandra Reactive Buffer Size and Time Config

### OUTBOUND_NOTIFICATION_BUFFER_MAX_SIZE
- Maximum size for outbound notification buffer.

### OUTBOUND_NOTIFICATION_BUFFER_MAX_TIME
- Maximum time for outbound notification buffer.

### OUTBOUND_CASSANDRA_BUFFER_MAX_SIZE
- Maximum size for outbound Cassandra buffer.

### OUTBOUND_CASSANDRA_BUFFER_MAX_TIME
- Maximum time for outbound Cassandra buffer.

## 37. Transformer ODK Postgres Pool Config

### FORMS_DB_POOL_INITIAL_SIZE
- Initial pool size for FormsDB.

### FORMS_DB_POOL_MAX_SIZE
- Maximum pool size for FormsDB.

## 38. TRANSACTION_AUTH_TOKEN

### AUTHORIZATION_KEY_TRANSACTION_LAYER
- Authorization key for the transaction layer.

## 39. Logging

### LOGS_FOLDER
- Folder for logs.

### ANALYSIS_TRIGGGER_FILE
- Analysis trigger file.

## 40. INBOUND CAFFINE

### CAFFINE_INVALIDATE_ENDPOINT
- Endpoint to invalidate Caffeine cache.

## 41. Broadcast Report

### BROADCAST_BOT_REPORT_ENDPOINT
- Endpoint for bot report in broadcast.

## 42. Web-Channel

### REACT_APP_TRANSPORT_SOCKET_URL
- URL for React app transport socket.

### REACT_APP_UCI_BOT_BASE_URL
- Base URL for the UCI bot in React app.

### REACT_APP_CHAT_HISTORY_URL
- URL for chat history in React app.

### REACT_APP_OTP_BASE_URL
- Base URL for OTP in React app.

### REACT_APP_OWNER_ID
- Owner ID in React app.

### REACT_APP_OwnerOrgId
- Owner organization ID in React app.

### REACT_APP_AUTH_TOKEN
- Authentication token in React app.

### REACT_APP_MOBILE
- Mobile number in React app.

### REACT_APP_Admin_Token
- Admin token in React app.

### REACT_APP_FILTER_LIST
- Filter list flag in React app.

### FORMSDB_HASURA_GRAPHQL_ADMIN_SECRET
- Admin secret for FormsDB Hasura GraphQL.

### FUSIONAUTH_CLIENT_SECRET
- Client secret for Fusionauth.

### FUSIONAUTH_APPLICATION_ID
- Application ID for Fusionauth.

### FUSIONAUTH_DATABASE_USER
- Database user for Fusionauth.

## 43. For Authentication from NL

### TRANSPORT_SOCKET_JWT_AUTH_URL
- JWT authentication URL for transport socket.
