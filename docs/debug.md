---
id: debug
title: Debug
sidebar_label: Debug
---

## 1. Overview

A service can be debugged via a IDE tool if you are using one to run the services or via checking the docker conatiner logs if you are using the docker image to run it.

## 2. Order of debugging services

To debug any issue or error, follow below order. We should check the service in this order as the message processes in this order only. 

1. Inbound
2. Orchestrator
3. Transformer
4. Outbound

A message will be first received in the inbound service to convert the message to XMessage format then it will be sent to the kafka topic which is being listened by the orchestrator service. To check the build/execution flow of the services [click chere](Build & Execution Flow.md). 

## 3. Debug on IDE Tool

When a service is started from a IDE tool, it will also show the logs associated with the service. If you are not receiving message reply or the service is not starting, please see the logs to check the issue, and work on the solution accordingly. 

You can also add debugger to the IDE tool to add break points in the code to check where the issues is generating from. If you don't know how to, check below videos for reference.

- How to add debugger in [Spring boot tool](https://www.youtube.com/watch?v=w5woL3znVuA)
- How to add debugger in [intellij](https://www.youtube.com/watch?v=1bCgzjatcr4)

## 4. Debug for Docker Image

If you have started the services using docker images, you should follow below steps to debug them.

#### 1. Check All Services logs

- Run command below to check logs for all the running services.

```docker-compose logs --follow --tail 10``` 

#### 2. Check Single Service logs

- If you are sure about the service which is giving error, we can check the logs for that particular service. First check the container id by running the command below. 

```docker ps```

It will return a list of running containers. Each container has a container id as shown in the image.

![](../media/docker-ps.png)

- To check logs for the container, run command

```docker logs --follow --tail 10 container_id```

To check the logs for kafka/redis etc services, you should follow the above steps.


### 5. Common mistakes or errors

1. Missing environment variables
2. Connection issues with kafka/cassandra/postgresql etc.
3. Port in use by another service.
4. Java version conflict