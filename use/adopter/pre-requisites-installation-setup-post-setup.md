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
3.  Install the requirements

    `pip3 install -r requirements.txt`

4. Fill in the environment variables in the `.env` file. Find out more about the `.env` [here](https://uci.sunbird.org/use/adopter/about-the-env).


5.  Run below command to download & start the services using docker.

    `python3 install.py {ENCRYPTION_KEY} {NETCORE_WHATSAPP_AUTH_TOKEN} {NETCORE_WHATSAPP_SOURCE} {NETCORE_WHATSAPP_URI}`

    The encryption key should be a strong string consisting uppercase, lowercase, numeric and special characters. For setting up a local demo, it can be anything. For example: 1234. The rest of the parameteres are optional, they are only required for the whatsapp flow. If you don't want to test it out, then you are good to go only with the encryption key. In that case, the final command comes out to be:

    `python3 install.py 1234`

    * If script fails on runtime then you should run below commands for continue run this script
      *   Stop the all services and volumes

          `docker-compose down -v`
      *   Remove the data directory

          `sudo rm -rf data`
      *   Run the script again

6.  Follow the instructions in the script.

**Note**: Please note this installation is based on a very standard and generalized use case. If your needs are not fulfilled with the current installation, please start scaling the individual services by using them in docker stack.

#### **After a Successful Docker Setup Completion**

1. Script execution time - **Around 30 minutes hours** (depends on your server)
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

       `docker-compose logs {service_name} --follow`

**Note**: If the services are updated on any server, use server's ip instead of localhost. Eg. http://143.112.x.x:9080

## **Post Setup**

1. You can start using FusionAuth Console using http://localhost:9011/ and create an Account, for managing users and what resources they are authorized to access.
2. For managing all the assesment data go on URL : http://localhost:15002/ and track all the tables and relation.

**Note**: If you face any difficulty in setting up UCI, please raise an issue [here](https://github.com/samagra-comms/docker-deploy/issues). All the issues that you raise will help us make UCI make better for adopters.