---
title: Poller
sidebar_title: Poller
permalink: /docs/poller
---

## Grunt Commands

All [grunt](https://gruntjs.com/getting-started) commands below belong to [service-poller](https://github.com/boxmls/service-poller)


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
