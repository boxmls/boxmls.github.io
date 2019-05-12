---
title: MLS API
sidebar_title: MLS API
permalink: /apps/mls-api
---

* Setup Elasticsearch mapping using the instructions in [boxmls/elasticsearch-mapping](https://github.com/boxmls/elasticsearch-mapping).
* Index existing property geoJSON areas using the instructions in [boxmls/geojson-mls-areas](https://github.com/boxmls/geojson-mls-areas).
* Setup RabbitMQ cluster. You can use any solution: third-party service such as [CloudAMQP](https://www.cloudamqp.com/) or build your own RabbitMQ cluster using  [docker-rabbitmq](https://github.com/boxmls/docker-rabbitmq) docker image.
* Fork [boxmls/service-rets-api](https://github.com/boxmls/service-rets-api) and start docker container `service-rets-api` using instructions in the repository.
* Fork [boxmls/service-mls-api](https://github.com/boxmls/service-mls-api) and setup your MLS configuration using instructions and examples below.
* Start docker container `service-mls-api` and complete setup of MLS by running required grunt commands. See instruction in [service-mls-api](https://github.com/boxmls/service-mls-api) repository.
* Start docker container from [boxmls/service-poller](https://github.com/boxmls/service-poller) using its instructions.

## Diagram

![image](https://user-images.githubusercontent.com/308489/57467645-93f77700-728b-11e9-875d-0fdb96215262.png)