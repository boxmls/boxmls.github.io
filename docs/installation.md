---
title: Installation
sidebar_title: Installation
permalink: /docs/installation
---

OpenBox platform is a multi-services application. It should be setup step by step.    
Please check out further pages to get familiar with required platforms and their installation and setup instructions and find open sourced codebase.

1. Setup Elasticsearch mapping using the instructions in [elasticsearch-mapping](https://boxmls.github.io/docs/apps/elasticsearch).
2. Index existing property geoJSON areas using the instructions in [geojson-mls-areas](https://boxmls.github.io/docs/apps/geojson-mls-areas).
3. Setup RabbitMQ cluster. You can use any solution: third-party service such as [CloudAMQP](https://www.cloudamqp.com/) or build your own RabbitMQ cluster using  [docker-rabbitmq](https://boxmls.github.io/docs/apps/rabbitmq) docker image.
4. Check out the [service-rets-api](https://boxmls.github.io/docs/apps/rets-api) and follow the instructions to setup service before next steps.
5. Check out the [service-mls-api](https://boxmls.github.io/docs/apps/mls-api) and follow the instructions to setup service and your `MLS` configs.
6. Check out the [service-poller](https://boxmls.github.io/docs/apps/poller) and follow the instructions to start and manage sync processes.

## Full process interaction

![image](https://user-images.githubusercontent.com/308489/57467645-93f77700-728b-11e9-875d-0fdb96215262.png)