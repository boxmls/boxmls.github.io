---
title: MLS Setup
sidebar_title: MLS Setup
permalink: /docs/apps/mls-api/mls-setup
---

## MLS configuration

* Create default MLS configuration files ( `handlebars`, `config.yaml` ). The example is available [here](https://github.com/boxmls/config-example/blob/master/mls/mock-mls). 
* Be sure that all `pm2` processes in `config.yaml` are disabled for now ( e.g. `poller.pm2.sync-property: false` ) to prevent starting `pm2` processes automatically.
* Setup RETS credentials ( `connector.config.credentials` ) and test them using Swagger UI of RETS API service.
* Setup RETS resources ( `connector.config.resources` ) and test them using Swagger UI of RETS API service.
* Index MLS configuration ( `MLS={name} grunt index` )
* Get all fields schemas ( `MLS={name} DEBUG=grunt grunt get-field-schema` )

### Notes

* MLS configuration is being retrieved from Elasticsearch. To apply changes in `config.yaml`, - be sure to re-index Elasticsearch data ( `MLS={name} grunt index` ).
* Run `grunt index` inside `service-mls-api` container to re-index MLS config(s).
* To setup RETS resources ( `connector.config.resources` ) [RETS M.D.](https://retsmd.com/index.php) service can be used. It's rendering RETS meta schema.

### Grunt Commands

All [grunt](https://gruntjs.com/getting-started) commands below belong to [service-mls-api](https://github.com/boxmls/service-mls-api/blob/master/gruntfile.js)

* `grunt index`. Extends and indexes all MLS configurations ( they are stored in Elasticsearch )
* `grunt get-field-schema`. Retrieves and indexes all RETS meta fields and their schemas. It's required for [Poller](https://github.com/boxmls/service-poller) sync and data normalization processes.

## Sync RETS RAW Data from MLS to ElasticSearch

* Start [Poller](https://github.com/boxmls/service-poller) docker container for the MLS.
* Enter to the container and run [grunt](https://gruntjs.com/) commands:
    * Sync property ( `TYPE=property DEBUG=grunt,sync,ret-request grunt sync` )
    * Sync agent ( `TYPE=agent DEBUG=grunt,sync,ret-request grunt sync` )
    * Sync office ( `TYPE=office DEBUG=grunt,sync,ret-request grunt sync` )

## Find, Prep, and Import areas (GeoJSON data) to Elasticsearch.

* Scour the internet to find neighborhood, city, zipcode, county data for the market.
* Possibly simplify GeoJSON zip codes, city, neighborhood data via [mapshaper.org](https://mapshaper.org/)
* Import all shapes into Elasticsearch by type ( e.g. `city`, `zipcode`, etc. )

If issues with indexing geojson data occurred:
* Repair problematic GeoJSON features if needed via QGIS v3.0's "Fix Geometry Script"
* Repair problematic GeoJSON features if needed from "right hand" error via https://mapster.me/right-hand-rule-geojson-fixer/
* Re-import repaired area by type ( e.g. `city`, `county` )

## Setup Handlebars views and normalize Raw RETS Data

* Agent handlebar mapping
* Office handlebar mapping
* Openhome handlebar mapping (depending on market)
* Tour handlebar mapping (depending on market)
* Property handlebar mapping ( note, list must be normalized last, when all other resources above are done ).