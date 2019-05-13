---
title: Service MLS API
sidebar_title: Service MLS API
permalink: /apps/mls-api
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

## MLS Configuration

All existing [MLS](https://en.wikipedia.org/wiki/Multiple_listing_service) configurations are stored in [service-mls-api](https://github.com/boxmls/service-mls-api/tree/master/static/config).

MLS Configuration includes:
* Configuration file (`config.yaml`)
* Template processors (`handlebars`)

### Configuration File

MLS configuration is stored in [YAML](https://en.wikipedia.org/wiki/YAML) file. It contains all necessary MLS details, options and credentials, which are being used by major of micro-services of BoxMLS application.

It includes the following conditional sections:
* Poller. Stores some specific options for [Poller Service](https://github.com/boxmls/service-poller)
* RETS connector configuration. It includes RETS credentials used by [RETS API service](https://github.com/boxmls/service-rets-api) for doing requests to [RETS](https://en.wikipedia.org/wiki/Real_Estate_Transaction_Standard) Provider. Also it describes the rules for resources, which are mostly needed for syncing the data by poller.

All available configuration options can be found in `config.yaml` [example](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/config.yaml).

### Template processors

BoxMLS stores all MLS data in two formats:
* Original MLS meta retrieved from RETS. Original meta can be different from MLS to MLS. This kind of data is used only for specific internal purposes and only by [Poller](https://github.com/boxmls/service-poller) service. The data is stored in Elasticsearch `rets` index ( e.g. `rets/agent` ).
* Internal standartized ( we also call it `normalized` ) data. The main format for all MLSs. [Poller](https://github.com/boxmls/service-poller) takes care of converting original MLS data to `normalized`. The data is stored in different indexes of Elasticsearch.

#### Templates

MLS original data is being normalized to internal standartized format by MLS [Poller Service](https://github.com/boxmls/service-poller) using [handlebarsjs](http://handlebarsjs.com/) templates. Every resource has its own [handlebarsjs](http://handlebarsjs.com/) template, which describes standartized JSON object.

Templates are stored in `static/config/{MLS}/handlebars/views` folder. See example [here](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views).

MLS can have up to five templates:
* [agent.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/agent.handlebars)
* [office.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/office.handlebars)
* [openhome.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/openhome.handlebars)
* [property.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/property.handlebars)
* [tour.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/tour.handlebars)

#### Helpers

Helpers is the set of handlebar functions used in templates. There are two types of Helpers:
* Common. All common helper functions are defined in Poller service and can be found in [lib/helpers](https://github.com/boxmls/service-poller/tree/master/lib/helpers) directory. 
* MLS Custom. Custom helpers are used and defined only by the specific MLS. They are defined and stored under MLS configuration on `mls-api`. See example [static/config/example/handlebars/helpers](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/helpers).

Note: common function is being re-declared by MLS custom function with the same name. E.g.: if MLS custom `stringToList` function will be defined it will be used instead of already existing common function with the same name.  

Common and MLS Custom helper functions may have the following set of files:
* `helper.js`. It's a common set of functions, which can be used by any [handlebarsjs](http://handlebarsjs.com/) template.
* `agent.js`. The specific set of functions used only in `agent.handlebars` template.
* `office.js`. The specific set of functions used only in `office.handlebars` template.
* `property.js`. The specific set of functions used only in `property.handlebars` template.
* `openhome.js`. The specific set of functions used only in `openhome.handlebars` template.
* `tour.js`. The specific set of functions used only in `tour.handlebars` template.


## MLS Setup

### MLS configuration

* Create default MLS configuration files ( `handlebars`, `config.yaml` ). The example is available [here](https://github.com/boxmls/service-mls-api/tree/master/static/config/example). 
* Be sure that all `pm2` processes in `config.yaml` are disabled for now ( e.g. `poller.pm2.sync-property: false` ) to prevent starting `pm2` processes automatically.
* Setup RETS credentials ( `connector.config.credentials` ) and test them using Swagger UI of RETS API service.
* Setup RETS resources ( `connector.config.resources` ) and test them using Swagger UI of RETS API service.
* Index MLS configuration ( `MLS={name} grunt index` )
* Get all fields schemas ( `MLS={name} DEBUG=grunt grunt get-field-schema` )

#### Notes

* MLS configuration is being retrieved from Elasticsearch. To apply changes in `config.yaml`, - be sure to re-index Elasticsearch data ( `MLS={name} grunt index` ).
* Run `grunt index` inside `service-mls-api` container to re-index MLS config(s).
* To setup RETS resources ( `connector.config.resources` ) [RETS M.D.](https://retsmd.com/index.php) service can be used. It's rendering RETS meta schema.

#### Grunt Commands

All [grunt](https://gruntjs.com/getting-started) commands below belong to [service-mls-api](https://github.com/boxmls/service-mls-api/blob/master/gruntfile.js)

* `grunt index`. Extends and indexes all MLS configurations ( they are stored in Elasticsearch )
* `grunt get-field-schema`. Retrieves and indexes all RETS meta fields and their schemas. It's required for [Poller](https://github.com/boxmls/service-poller) sync and data normalization processes.

### Sync RETS RAW Data from MLS to ElasticSearch

* Start [Poller](https://github.com/boxmls/service-poller) docker container for the MLS.
* Enter to the container and run [grunt](https://gruntjs.com/) commands:
    * Sync property ( `TYPE=property DEBUG=grunt,sync,ret-request grunt sync` )
    * Sync agent ( `TYPE=agent DEBUG=grunt,sync,ret-request grunt sync` )
    * Sync office ( `TYPE=office DEBUG=grunt,sync,ret-request grunt sync` )

#### Find, Prep, and Import areas (GeoJSON data) to Elasticsearch.

* Scour the internet to find neighborhood, city, zipcode, county data for the market.
* Possibly simplify GeoJSON zip codes, city, neighborhood data via [mapshaper.org](https://mapshaper.org/)
* Import all shapes into Elasticsearch by type ( e.g. `city`, `zipcode`, etc. )

If issues with indexing geojson data occurred:
* Repair problematic GeoJSON features if needed via QGIS v3.0's "Fix Geometry Script"
* Repair problematic GeoJSON features if needed from "right hand" error via https://mapster.me/right-hand-rule-geojson-fixer/
* Re-import repaired area by type ( e.g. `city`, `county` )

#### Setup Handlebars views and normalize Raw RETS Data

* Agent handlebar mapping
* Office handlebar mapping
* Openhome handlebar mapping (depending on market)
* Tour handlebar mapping (depending on market)
* Property handlebar mapping ( note, list must be normalized last, when all other resources above are done ).

## Support

Do you have any questions. Please, visit [Support](https://boxmls.github.io/support) page for consulting and help.