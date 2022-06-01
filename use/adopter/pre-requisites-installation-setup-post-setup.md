# Pre-requisites, Installation Setup, Post Setup

[Pre-requisites](#pre-requisites)

[Installation](#installation-setup)

[Post Setup](#post-setup)

# Pre-requisites

1. Install Docker if not already installed. [Click Here](https://docs.docker.com/engine/install/ubuntu/)
2. Infra :&#x20;
   * 8 CPU Cores
   * 16 GB RAM
   * 20 GB Storage
   * Server Fixed/Static IP
   * Preferred OS - Linux
3. Basic familiarity is required with:
   * Terminal & its commands
   * Docker
   * Server
   * Rest apis
4. Please make sure all of the ports mentioned used in the [file](https://github.com/samagra-comms/docker-deploy/blob/main/docs/ports.md) are open & are not being used by any other service on the server.

# Installation Setup

1.  Take a clone of this repository.

    git clone https://github.com/samagra-comms/docker-deploy.git
2.  Go to the folder

    `cd docker-deploy`
3. Contact the [administrator](https://github.com/samagra-comms/docker-deploy#contact-administrator) for `ENCRYPTION_KEY` and update its value in [.env](https://github.com/samagra-comms/docker-deploy/blob/main/.env) file.
4. To get the messages from any service provider say netcore/gupshup, contact their support team, and ask them to add your ip with netcore/gupshup adapter url For Netcore: ip:inbound\_extrenal\_port/netcore/whatsApp (Eg. - 143.112.x.x:9080/netcore/whatsApp) For Gupshup: ip:inbound\_external\_port/gupshup/whatsApp (Eg. - 143.112.x.x:9080/gupshup/whatsApp)
5.  If you want to use the Netcore service, update below details in the [.env](https://github.com/samagra-comms/docker-deploy/blob/main/.env) file.

    ```
    NETCORE_WHATSAPP_AUTH_TOKEN = # Authentication Token 
    NETCORE_WHATSAPP_SOURCE = # Source ID for sending messages to Netcore
    NETCORE_WHATSAPP_URI = # Netcore API Base URL
    ```
6.  Run the below command to download and start the services using docker.

    `bash install.sh`
7. If asked, **Would you like to share anonymous usage data about this project with the Angular Team at Google under Google’s Privacy Policy at** [**https://policies.google.com/privacy**](https://policies.google.com/privacy)**? For more details and how to change this setting, see** [**http://angular.io/analytics**](http://angular.io/analytics)**.**, Press **y**.
8. This script will download all the service images & start the services.
9. If you change anything in [.env](https://github.com/samagra-comms/docker-deploy/blob/main/.env) file, you will have to stop the services, then restart them.
   *   Stop all services:

       `docker-compose -f docker-compose.yml down`
   *   Start all services:

       `docker-compose -f docker-compose.yml up -d`

**After a successful setup completion**

1. Script maximum execution time - **Around 2 hours**
2. On a successful run of the script, you will get the below services:
   * Inbound Service: [http://localhost:9080](http://localhost:9080/)
   * Orchestrator Service: [http://localhost:8686](http://localhost:8686/)
   * Transformer Service: [http://localhost:9091](http://localhost:9091/)
   * Outbound Service: [http://localhost:9090](http://localhost:9090/)
   * Broadcast Transformer Service: [http://localhost:9093](http://localhost:9093/)
   * Campaign Service: [http://localhost:9999](http://localhost:9999/)
   * Kafka UI: [http://localhost:18080/ui/docker-kafka-server/topic](http://localhost:18080/ui/docker-kafka-server/topic)
   * Bot DB Hasura UI: [http://localhost:15003/console/login](http://localhost:15003/console/login)
   * Fusion Auth: [http://localhost:9011](http://localhost:9011/)
   * ODK: [http://localhost:8080/Aggregate.html#management/forms](http://localhost:8080/Aggregate.html#management/forms)
   * Chat Frontend: [http://localhost:9098](http://localhost:9098/)
   * Admin Console: [http://localhost:9097](http://localhost:9097/)
3.  If you want to check all the services logs, use the below command.

    `docker-compose -f docker-compose.yml --follow --tail 10`
4. If you want to check logs for a specific service, follow the below flow.
   *   Check service container id:

       `docker ps -a`
   *   Copy the container id from the list for the service you want to see the logs for, and use it in the below command.

       `docker --follow --tail 10 container_id`

**Note**: If the services are updated on any server, use the server's IP instead of localhost. E.g. [http://143.112.x.x:9080](http://143.112.x.x:9080/)

# Post Setup

1. [Tracking Tables](https://hasura.io/docs/latest/graphql/core/databases/postgres/schema/using-existing-database.html#step-1-track-tables-views). Go to the url [http://localhost:15003/console/data/default/schema/public](http://localhost:15003/console/data/default/schema/public) and track all tables and relations. The admin secret can be controlled using this [line](https://github.com/samagra-comms/docker-deploy/blob/10bdbc4b837a61f74a1270ce53467b15f63d182d/.env#L67)
2. Adding default data for transformers
   * Go to [http://localhost:15003/console/data/default/schema/public](http://localhost:15003/console/data/default/schema/public) and track all of the items one by one.
   *   In the sidebar click on the SQL button and add the following commands and run.

       ```
       INSERT INTO service ("id", "type", "config")
       VALUES ('94b7c56a-6537-49e3-88e5-4ea548b2f075', 'odk', '{"cadence": { "retries": 0, "timeout": 60, "concurrent": true, "retries-interval": 10 }, "credentials": { "vault": "samagra", "variable": "samagraMainODK" } }');
       INSERT INTO adapter ("id", "provider", "channel", "config", "name") 
       VALUES ('44a9df72-3d7a-4ece-94c5-98cf26307324', 'WhatsApp', 'gupshup', '{ "2WAY": "2000193033", "phone": "9876543210", "HSM_ID": "2000193031", "credentials": { "vault": "samagra", "variable": "gupshupSamagraProd" } }', 'SamagraProd');
       INSERT INTO adapter ("id", "provider", "channel", "config", "name") 
       VALUES ('44a9df72-3d7a-4ece-94c5-98cf26307323', 'WhatsApp', 'Netcore', '{ "phone": "912249757677", "credentials": { "vault": "samagra", "variable": "netcoreUAT" } }', 'SamagraNetcoreUAT');
       INSERT INTO transformer ("name", "tags", "config", "id", "service_id") 
       VALUES ('SamagraODKAgg', array['ODK'], '{}', 'bbf56981-b8c9-40e9-8067-468c2c753659', '94b7c56a-6537-49e3-88e5-4ea548b2f075'); 
       ```
3. Now we can start Sent/Receive messages from channel.
4. You can start using the FusionAuth Console by using the [link](http://localhost:9011/) and create an Account, for managing users and what resources they are authorized to access.
5. For managing all the assessment data, go to URL: [http://localhost:15002/](http://localhost:15002/) and track all the tables and relations using [token](https://github.com/samagra-comms/docker-deploy/blob/main/docker-compose.yml#L363).
