---
title: Service MLS API
sidebar_title: Service MLS API
permalink: /docs/apps/mls-api
---

* The service which manages all MLS configurations.
* All MLS configurations should be stored in `yaml` format at the own private repository. [Here](https://github.com/boxmls/config-example) you can find an example of MLS configurations.
* Indexing MLS configuration proceeds automatically on container start. On every update of MLS configuration, the data must be re-indexed, so to (re)index Elasticsearch data, restart the container.

## OpenSource codebase

Please visit [github repository](https://github.com/boxmls/service-mls-api). There you will able to check out codebase, contribute development process 
or make a fork to have your own development workflow.

## Installation

* Make a fork of the current repository into your user/organization
* Clone repository into your local
```
git clone https://github.com/boxmls/service-mls-api -b develop
```
* Setup environment variables before start container. You can check the list of needed variables at the `docker-composer.yml` 
* Run the command provided bellow to up the `docker` container.
```
docker-compose up --build --renew-anon-volumes
```

* Follow out [MLS Configuration](https://boxmls.github.io/docs/apps/mls-api/mls-configuration) to configure service-mls-api
* Follow out [MLS Setup](https://boxmls.github.io/docs/apps/mls-api/mls-setup) to configure mls clients config 

## Support

Do you have any questions. Please, visit [Support](https://boxmls.github.io/support) page for consulting and help.