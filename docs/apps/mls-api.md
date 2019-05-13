---
title: Service MLS API
sidebar_title: Service MLS API
permalink: /docs/apps/mls-api
---

* The service which manages all MLS configurations.
* All MLS configurations are stored in `yaml` format in `static/mls` directory.
* On every update of MLS configuration, the data must be re-indexed. To (re)index Elasticsearch data, run `grunt index`.

## OpenSource codebase

Please visit [github repository](https://github.com/boxmls/service-mls-api). There you will able to check out codebase, contribute development process 
or make a fork to have your own development workflow.

## Docker Start 

To run in development mode we need to change the name and volume-mount our repository for SSH deployment to work:

```
bin/build.sh; docker rm -fv mls-api.$(git rev-parse --symbolic-full-name --abbrev-ref HEAD); docker run -itd \
  --name=mls-api.$(git rev-parse --symbolic-full-name --abbrev-ref HEAD) \
  --label="git.owner=boxmls" \
  --label="git.name=service-mls-api" \
  --label="git.branch=$(git rev-parse --symbolic-full-name --abbrev-ref HEAD)" \
  --env=NODE_ENV=development \
  --env=GIT_OWNER=boxmls \
  --env=GIT_NAME=service-mls-api \
  --env=GIT_BRANCH=$(git rev-parse --symbolic-full-name --abbrev-ref HEAD) \
  --env=ES_ADDRESS="${COREOS_PRIVATE_IPV4}:9200" \
  --env=NODE_PORT=8080 \
  --memory=4g \
  --publish=8080 \
  --volume=$(pwd):/opt/sources/boxmls/service-mls-api \
  boxmls/service-mls-api:master
```


## Installation

* Make a fork of the current repository into your user/organization
* Update config data with needed values [package.json](https://github.com/boxmls/service-mls-api/blob/master/package.json#L14-L17)
* Follow out [MLS Configuration](https://boxmls.github.io/docs/apps/mls-api/mls-configuration) to configure service-mls-api
* Follow out [MLS Setup](https://boxmls.github.io/docs/apps/mls-api/mls-setup) to configure mls clients config 

## Support

Do you have any questions. Please, visit [Support](https://boxmls.github.io/support) page for consulting and help.