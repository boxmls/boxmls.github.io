---
title: GeoJSON MLS Areas Setup
sidebar_title: GeoJSON MLS Areas
permalink: /apps/geojson-mls-areas
---
![image](https://user-images.githubusercontent.com/308489/57512890-9acacc00-7315-11e9-854f-ad77da4d2742.png)

# GeoJSON MLS Areas

Standartize GeoJSON areas to be used with BoxMLS listings (properties).

## Areas Setup

GeoJSON MLS areas are stored in Elasticsearch. On adding new MLS market, setup of new GeoJSON areas may be needed:

* Fork the current repository, since it requires to update `static/areas` data.
* Scour the internet to find `neighborhood`, `city`, `zipcode`, `county` data for the market.
* Possibly simplify GeoJSON `zipcode`, `city`, `neighborhood` data via [mapshaper.org](https://mapshaper.org/)
* Clone forked repository to your local environment. 
* Save geojson file to `static/areas` directory. Use the following name pattern: `{st}_{state}_{type}.geojson` 
* Import all shapes into Elasticsearch by type ( e.g. `city`, `zipcode`, etc. ). See grunt task below.

### Grunt

```
INDEX=area-10m STATE={ST} TYPE=city grunt importAreas
```

Indexes areas (shapes) from `static/data/{st}_{state}_{type}.geojson`.

Examples:

```
DEBUG=grunt INDEX=area-10m STATE=AZ TYPE=city grunt importAreas
DEBUG=grunt INDEX=area-10m STATE=AZ TYPE=neighborhood grunt importAreas
DEBUG=grunt INDEX=area-10m STATE=CO TYPE=zipcode grunt importAreas
```

## Area types

The following areas types are being 

* `sub_neighborhood`. Sub-Neighborhood.
* `neighborhood`. Neighborhood
* `county`. County
* `city`. City
* `state`. State
* `zipcode`. Zip

### Levels

`properties.boundary.level` is used to store area level (see GeoJSON structure below). GeoJSON levels to be used:

* `0` Sub-Neighborhood.
* `1` Neighborhood
* `3` City
* `4` County
* `5` State
* `7` Zip

### Suggest Priority

`_suggestPriority` meta (see GeoJSON structure below) is optional. It's used only for sorting purposes on search. You can ignore it. 

E.g.: in BoXMLS, the autocomplete (autosuggest) response is being sorted by `_suggestPriority` meta

* `_suggestPriority` = `10` `area/city`
* `_suggestPriority` = `20` `area/sub_neighborhood`
* `_suggestPriority` = `30` `area/neighborhood`
* `_suggestPriority` = `50` `area/county`
* `_suggestPriority` = `70` `area/state`
* `_suggestPriority` = `80` `area/zipcode`

Indexes areas from static/areas/{st}_{state}_{type}.json

* `st` Short state name
* `state` State name
* `type` Type of area

## GeoJSON structure

Example

```
{
  "type": "Feature",
  "properties": {
    "boundary": {
      "level": 7,
      "levelType": "zipcode",
      "name": "93265",
      "levelTypeLabel": "Zip"
    },
    "location": {
      "state": "California",
      "countyFips": 6,
      "zipCodes": [
        "93265"
      ]
    },
    "uniqueID": 700445
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": []
  },
  "center": [
    "+36.2459265",
    "-118.6931383"
  ],
  "title": "93265",
  "_suggestPriority": 80
}
```

# Support

Do you have any questions. Please, visit [Support](https://boxmls.github.io/support) page for consulting and help.
content.com/308489/57467645-93f77700-728b-11e9-875d-0fdb96215262.png)