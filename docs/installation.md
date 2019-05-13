---
title: Installation
sidebar_title: Installation
permalink: /docs/installation
---

* Setup Elasticsearch mapping using the instructions in [elasticsearch-mapping](https://boxmls.github.io/apps/elasticsearch).
* Index existing property geoJSON areas using the instructions in [geojson-mls-areas](https://boxmls.github.io/apps/geojson-mls-areas).
* Setup RabbitMQ cluster. You can use any solution: third-party service such as [CloudAMQP](https://www.cloudamqp.com/) or build your own RabbitMQ cluster using  [docker-rabbitmq](https://boxmls.github.io/apps/rabbitmq) docker image.
* Check out the [service-rets-api](https://boxmls.github.io/apps/rets-api) and follow the instructions to setup service before next steps.
* Check out the [service-mls-api](https://github.com/apps/mls-api) and follow the instructions to setup service and your `MLS` configs.
* Check out the [service-poller](https://github.com/apps/poller) and follow the instructions to start and manage sync processes.

## Diagram

![image](https://user-images.githubusercontent.com/308489/57467645-93f77700-728b-11e9-875d-0fdb96215262.png)