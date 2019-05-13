---
title: Service Poller
sidebar_title: Service Poller
permalink: /apps/service-poller
---

A Docker service for intelligently polling an MLS for changes and emitting updates.

## OpenSource codebase

Please visit [github repository](https://github.com/boxmls/service-poller). There you will able to check out codebase, contribute development process 
or make a fork to have your own development workflow.

## Features
* Responsible for mapping RETS data to JSON RESO standards
* Sync RETS data almost in real time ( e.g.: properties may be synced every 10 seconds, - depends on [MLS configuration](https://github.com/boxmls/service-mls-api) ).
* One Poller Service is One MLS MLS market.
* Scales infinitely.  Easily update mapping in seconds
* Syncs 100% of the data from the MLS

## Supported RETS resources

* `Property`. Required.
* `Agent`. Optional.
* `Office`. Optional.
* `OpenHome`. Optional.
* `Tour`. Optional.

## Requirements

Docker service Poller is working with the following services in symbiosis:

* [service-mls-api](https://github.com/boxmls/service-mls-api). Docker service.
* [service-rets-api](https://github.com/boxmls/service-rets-api). Docker service.
* [Elasticsearch](https://www.elastic.co/) cluster and [defined es mapping](https://github.com/boxmls/elasticsearch-mapping).
* [RabbitMQ](https://www.rabbitmq.com) cluster.

## Installation

Before Poller setup, be sure that:

* [Elasticsearch](https://www.elastic.co/) cluster is setup with all required indexes and mapping.
* [RabbitMQ](https://www.rabbitmq.com) cluster is up.
* [service-mls-api](https://github.com/boxmls/service-mls-api) docker service is up.
* [service-rets-api](https://github.com/boxmls/service-mls-api) docker service is up.

## Starting service

* Fork the current repository.
* Setup [gce-amqp-cluster-client-config.json](https://github.com/boxmls/service-poller/blob/master/gce-amqp-cluster-client-config.json), which is required for connecting to RabbitMQ node in your GCE Cluster. See [node-gce-amqp-cluster-client](https://github.com/UsabilityDynamics/node-gce-amqp-cluster-client)
* Start Docker container. See below.

### Docker Runtime

* To run in development mode we need to change the name and volume-mount to our repository for SSH deployment to work
* `MLS_SYS` env must be defined and name of poller should be suffixed with `MLS_SYS` too.

```
docker rm -fv poller-${MLS_NAME}.$(git rev-parse --symbolic-full-name --abbrev-ref HEAD); docker run -itd  \
  --name=poller-${MLS_NAME}.$(git rev-parse --symbolic-full-name --abbrev-ref HEAD) \
  --label="git.owner=boxmls" \
  --label="git.name=service-poller" \
  --label="git.branch=$(git rev-parse --symbolic-full-name --abbrev-ref HEAD)" \
  --env=MLS_SYS=${MLS_NAME} \
  --env=GIT_OWNER=boxmls \
  --env=GIT_NAME=service-poller \
  --env=GIT_BRANCH=$(git rev-parse --symbolic-full-name --abbrev-ref HEAD) \
  --env=NODE_ENV=development \
  --env=ES_ADDRESS=${ES_ADDRESS_HOST} \
  --memory=8g \
  --publish=8080 \
  --volume=$(pwd):/opt/sources/boxmls/service-poller \
  boxmls/service-poller:latest
```

To use custom MLS API container for development purposes, custom MLS API host `MLS_API_HOST` can be set:

```
  --env=MLS_API_HOST=${MLS_API_HOST}
```

## PM2 Processes

The Docker Poller is built on [nodejs](https://nodejs.org/en/) and uses the following [PM2](http://pm2.keymetrics.io/) processes:

* `sync-property`. It is watching for RETS `Property` resource updates depending on property last modified date time.
* `sync-property-media`. Optional. It is watching for RETS `Property` resource updates depending on media last modified date time.
* `sync-agent`. It is watching for RETS `Agent` resource updates depending on agent last modified date time.
* `sync-office`. It is watching for RETS `Office` resource updates depending on office last modified date time.
* `sync-openhome`. Optional. It is synchronizing RETS `OpenHome` resource every 3 hours by default.
* `sync-tour`. Optional. It is synchronizing RETS `OpenHome` resource every 3 hours by default.
* `sync-deep`. RabbitMQ consumer, which is watching for `agent`, `office`, `openhome` and `tour` updates and updates associated properties.
* `media-fetch`. RabbitMQ consumer, which is watching for `property` updates and updates its images if needed.
* `rets-proxy-server`. All requests to RETS API are being proxied via this internal proxy server, which queues them to prevent RETS Session errors due to requests limits.

# Grunt Commands

#### **mls-config**

Updates MLS configuration (flush caches and re-upload handlebars). 

```
CACHE_FLUSH=true DEBUG=mls grunt mls-config
```

It Requests MLS Configuration for particular defined MLS. If `MLS_SYS` is not defined, - error will be returned.

#### **sync**

Sync all (property,office,agent,openhome,tour) RETS RAW data.

To sync all data.

```
DEBUG=grunt,sync,model:mls,ret-request nohup grunt --stack-size=4096 sync &> sync.out.log&
```

To sync only specific type:

```
TYPE=property DEBUG=grunt,sync,model:mls,ret-request nohup grunt --stack-size=4096  sync &> sync.property.out.log&
TYPE=agent DEBUG=grunt,sync,model:mls,ret-request nohup grunt --stack-size=4096 sync &> sync.agent.out.log&
TYPE=office DEBUG=grunt,sync,model:mls,ret-request nohup grunt --stack-size=4096 sync &> sync.office.out.log&
```

#### **normalize**

Scrolls all or specific items of the provided type (e.g. 'agent') and (re)normalize them.

Parameters:
* TYPE. Required. Enums: property, agent, office, openhome, tour
* QUERY. Optional. Elasticsearch query
* SIZE. Optional. Default: 1

```
DEBUG=* TYPE=property SIZE=10 nohup grunt --stack-size=4096 normalize &> normalize.property.out.log&
DEBUG=* TYPE=agent SIZE=10 nohup grunt --stack-size=4096 normalize &> normalize.agent.out.log&
DEBUG=* TYPE=office SIZE=10 nohup grunt --stack-size=4096 normalize &> normalize.office.out.log&
```

To normalize the item by its ID ( note: you can re-normalize ONLY data tied to the current `MLS_SYS` ):

```
QUERY="_id:sfar-380147" DEBUG=* TYPE=property grunt normalize
```

## Support

Do you have any questions. Please, visit [Support](https://boxmls.github.io/support) page for consulting and help.
