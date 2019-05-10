---
title: Tours
sidebar_title: Tours
permalink: /docs/mapping-data/tours
---
Tours data mapping
```
{
    "_listingId": "{{fetchListingID ListingRid "ListingRid"}}",
    "_mlsListingId": "{{ListingRid}}",
    "_eventType": "original",
    "startDateTime": "{{startDateTimestamp TourDate TimePeriod}}",
    "endDateTime": "{{endDateTimestamp TourDate TimePeriod}}",
    "parking": {{#number}}{{Parking}}{{/number}},
    "units": {{#number}}{{Units}}{{/number}},
    "firstTimeOnTour": {{#bool}}{{NewRepeat}}{{/bool}},
    "lockboxOnProperty": {{#bool}}{{LockBox}}{{/bool}},
    "comments": "{{Comments}}",
    "tourCanceled": {{#bool}}{{Cancelled}}{{/bool}}
}
```