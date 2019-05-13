---
title: MLS Configuration
sidebar_title: MLS Configuration
permalink: /docs/apps/mls-api/mls_configuration
---

All existing [MLS](https://en.wikipedia.org/wiki/Multiple_listing_service) configurations are stored in [service-mls-api](https://github.com/boxmls/service-mls-api/tree/master/static/config).

MLS Configuration includes:
* Configuration file (`config.yaml`)
* Template processors (`handlebars`)

## Configuration File

MLS configuration is stored in [YAML](https://en.wikipedia.org/wiki/YAML) file. It contains all necessary MLS details, options and credentials, which are being used by major of micro-services of BoxMLS application.

It includes the following conditional sections:
* Poller. Stores some specific options for [Poller Service](https://github.com/boxmls/service-poller)
* RETS connector configuration. It includes RETS credentials used by [RETS API service](https://github.com/boxmls/service-rets-api) for doing requests to [RETS](https://en.wikipedia.org/wiki/Real_Estate_Transaction_Standard) Provider. Also it describes the rules for resources, which are mostly needed for syncing the data by poller.

All available configuration options can be found in `config.yaml` [example](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/config.yaml).

## Template processors

BoxMLS stores all MLS data in two formats:
* Original MLS meta retrieved from RETS. Original meta can be different from MLS to MLS. This kind of data is used only for specific internal purposes and only by [Poller](https://github.com/boxmls/service-poller) service. The data is stored in Elasticsearch `rets` index ( e.g. `rets/agent` ).
* Internal standartized ( we also call it `normalized` ) data. The main format for all MLSs. [Poller](https://github.com/boxmls/service-poller) takes care of converting original MLS data to `normalized`. The data is stored in different indexes of Elasticsearch.

### Templates

MLS original data is being normalized to internal standartized format by MLS [Poller Service](https://github.com/boxmls/service-poller) using [handlebarsjs](http://handlebarsjs.com/) templates. Every resource has its own [handlebarsjs](http://handlebarsjs.com/) template, which describes standartized JSON object.

Templates are stored in `static/config/{MLS}/handlebars/views` folder. See example [here](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views).

MLS can have up to five templates:
* [agent.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/agent.handlebars)
* [office.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/office.handlebars)
* [openhome.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/openhome.handlebars)
* [property.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/property.handlebars)
* [tour.handlebars](https://github.com/boxmls/service-mls-api/tree/master/static/config/example/handlebars/views/tour.handlebars)

### Helpers

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
