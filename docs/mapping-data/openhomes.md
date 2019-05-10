---
title: Open Homes
sidebar_title: Open Homes
permalink: /docs/mapping-data/openhomes
---
Open Homes data mapping
```
{
"_listingId": "{{fetchListingID LIST1 "LIST_1"}}",
"_mlsListingId": "{{LIST1}}",
"_eventType": "original",
"startDateTime": "{{EVENT100}}",
"endDateTime": "{{EVENT200}}",
"comments": "{{OPEN_HOUSE_COMMENT}}",
"openHouseRID": {{#number}}{{simplifyOpenHouseRID EVENT0}}{{/number}}
}
```