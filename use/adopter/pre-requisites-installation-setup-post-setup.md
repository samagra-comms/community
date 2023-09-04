# Pre-requisites, Installation Setup, Post Setup

## Pre-requisites, Installation Setup, Post Setup

[Pre-requisites](pre-requisites-installation-setup-post-setup.md#pre-requisites)

[Installation](pre-requisites-installation-setup-post-setup.md#installation-setup)

[Post Setup](pre-requisites-installation-setup-post-setup.md#post-setup)

## Pre-requisites

1. Install Docker if not already installed. [Click Here](https://docs.docker.com/engine/install/ubuntu/)
2. Infra :
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

## Installation Setup

1.  Take clone of this repository.

    `git clone https://github.com/samagra-comms/docker-deploy.git`
2.  Go to folder

    `cd docker-deploy`
3.  Run below command to download & start the services using docker.

    `bash installv2.sh`

    * If script is fail on runtime then you should run below commands for continue run this script
      *   Stop the all services

          `docker-compose down`
      *   Start the all services

          `docker-compose up -d`
4. While the script is running, You will be asked to enter encryption key, contact the administrator and get this.
5. If you are asked to enter Netcore Whatsapp Auth Token, Source, URI, enter the details if you have any else press enter. Click here to check the whatsapp configuration for netcore.

**Note**: Please note this installation is just the first step. If your needs are not fulfilled with the current installation, please start scaling the individual services by using them in docker stack.

#### **After a Successful Docker Setup Completion**

1. Script execution time - **Around 1 hours** (its depends on your server)
2.  On a successful run of the script you will get below services:

    * Inbound Service: http://localhost:9080/health
    * Orchestrator Service: http://localhost:8686/health
    * Transformer Service: http://localhost:9091/health
    * Outbound Service: http://localhost:9090/health
    * Broadcast Transformer Service: http://localhost:9093/health
    * Campaign Service: http://localhost:9999
    * Kafka UI: http://localhost:18080/ui/docker-kafka-server/topic
    * Bot DB Hasura UI: http://localhost:15003/console/login
    * Fusion Auth: http://localhost:9011
    * ODK: http://localhost:8080/Aggregate.html#management/forms
    * Chat Frontend: http://localhost:9098
    * Admin Console: http://localhost:9097

    **Note**: If above mentioned url's are not working then restart the all docker services or check the docker container logs.
3. If you want to check logs for a specific service, follow below flow.
   *   Show all docker containers

       `docker ps -a`
   *   Copy the container id from the list for the service you want to see the logs for, and use it in below command.

       `docker --follow --tail 10 container_id`

**Note**: If the services are updated on any server, use server's ip instead of localhost. Eg. http://143.112.x.x:9080

## **Post Setup**

1. **Tracking Tables/Views :** When the script is executed, tables will be created for bot schema via migrations. These tables can be assessed in Hasura. We need to track these tables to expose these tables to Hasura GraphQL. [Click to view](https://hasura.io/docs/latest/graphql/core/databases/postgres/schema/using-existing-database/) tracking overview.
   * Go to the url http://localhost:15003/console/data/default/schema/public
   * The admin secret can be controlled using this [link](https://github.com/samagra-comms/docker-deploy/blob/10bdbc4b837a61f74a1270ce53467b15f63d182d/.env#L67)
   * Go to http://localhost:15003/console/data/default/schema/public and track all of the items one by one.
2. After the tables are created, some default data for adapter, transformers etc should be added. These will be used later while creating a bot. Please follow below step:
   *   In the sidebar click on the SQL button and add the following commands and run.

       ```sql
        INSERT INTO "Service" ("id", "updatedAt", "type", "config")
        VALUES ('94b7c56a-6537-49e3-88e5-4ea548b2f075', NOW(), 'odk', '{"cadence": { "retries": 0, "timeout": 60, "concurrent": true, "retries-interval": 10 }, "credentials": { "vault": "samagra", "variable": "samagraMainODK" } }');
        INSERT INTO "Adapter" ("id", "updatedAt", "provider", "channel", "config", "name")
        VALUES ('44a9df72-3d7a-4ece-94c5-98cf26307324', NOW(), 'gupshup', 'WhatsApp', '{ "2WAY": "2000193033", "phone": "9876543210", "HSM_ID": "2000193031", "credentials": { "vault": "samagra", "variable": "gupshupSamagraProd" } }', 'SamagraProd');

        INSERT INTO "Adapter" ("id", "updatedAt", "provider", "channel", "config", "name")
        VALUES ('44a9df72-3d7a-4ece-94c5-98cf26307323', NOW(), 'Netcore', 'WhatsApp', '{ "phone": "912249757677", "credentials": { "vault": "samagra", "variable": "netcoreUAT" } }', 'SamagraNetcoreUAT');

        INSERT INTO "Adapter" ("id", "updatedAt", "provider", "channel", "config", "name")
        VALUES ('64036edb-e763-44b1-99b8-37b6c7b292c5', NOW(), 'gupshup', 'sms', '{"2WAY":"2000193033","phone":"9876543210","HSM_ID":"2000193031","credentials":{"vault":"samagra","variable":"gupshupSamagraProd"}}', 'SamagraGupshupSms');

        INSERT INTO "Adapter" ("id", "updatedAt", "provider", "channel", "config", "name")
        VALUES ('4e0c568c-7c42-4f88-b1d6-392ad16b8546', NOW(), 'cdac', 'sms', '{"2WAY":"2000193033","phone":"9876543210","HSM_ID":"2000193031","credentials":{"vault":"samagra","variable":"gupshupSamagraProd"}}', 'SamagraCdacSms');

        INSERT INTO "Adapter" ("id", "updatedAt", "provider", "channel", "config", "name")
        VALUES ('2a704e82-132e-41f2-9746-83e74550d2ea', NOW(), 'firebase', 'web', '{ "credentials": { "vault": "samagra", "variable": "uci-firebase-notification" } }', 'SamagraFirebaseWeb');

        INSERT INTO "Transformer" ("name", "tags", "config", "id", "serviceId", "updatedAt")
        VALUES ('SamagraODKAgg', array['ODK'], '{}', 'bbf56981-b8c9-40e9-8067-468c2c753659', '94b7c56a-6537-49e3-88e5-4ea548b2f075', NOW());

        INSERT INTO "Transformer" ("name", "tags", "config", "id", "serviceId", "updatedAt")
        VALUES ('SamagraBroadcast', array['broadcast'], '{}', '774cd134-6657-4688-85f6-6338e2323dde', '94b7c56a-6537-49e3-88e5-4ea548b2f075', NOW());

        INSERT INTO "Transformer" ("name", "tags", "config", "id", "serviceId", "updatedAt")
        VALUES ('SamagraGeneric', array['generic'], '{}', '0832ca13-c698-4234-8070-b5f708bc0b1a', '94b7c56a-6537-49e3-88e5-4ea548b2f075', NOW());
       ```
3. Now we can start Sent/Receive messages using uci web channel http://localhost:9098/ but first you should create a bot for conversation after that you will send starting message.
4. You can start using FusionAuth Console using http://localhost:9011/ and create an Account, for managing users and what resources they are authorized to access.
5. For managing all the assesment data go on URL : http://localhost:15002/ and track all the tables and relation using [token](https://github.com/samagra-comms/docker-deploy/blob/main/docker-compose.yml#L363).
