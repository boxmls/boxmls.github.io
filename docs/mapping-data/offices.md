---
title: Offices
sidebar_title: Offices
permalink: /docs/mapping-data/offices
---
Offices data mapping
```
{
  "_modified": {{#date}}{{TIMESTAMP}}{{/date}},
  "officeName": "{{OFFICE_2}}",
  "officeMLSID": "{{OFFICE_0}}",
  "officeNumber": "{{OFFICE_15}}",
  "officeLongName": "{{OFFICE_2}}",
  "officeType": "",
  "officeActive": {{#bool}}{{STATUS}}{{/bool}},
  "masterOfficeId": "",
  "mainOfficeId": "{{OFFICE_17}}",
  "nrdsid": "{{OFFICE_1}}",
  "idxOpt": {{getIdxOpt IDXOPT}},
  "vow": "",
  "branchType": "",
  "branchNum": "",
  "boardID": "{{OFFICE_16}}",
  "contact": {
    "primaryPhone": {{#if OFFICE_18 }}"{{getPhone OFFICE_18}}"{{else}}null{{/if}},
    "primaryPhoneExt": "",
    "otherPhone": {{#if OFFICE_3 }}"{{getPhone OFFICE_3}}"{{else}}null{{/if}},
    "fax": {{#if OFFICE_6 }}"{{getPhone OFFICE_6}}"{{else}}null{{/if}},
    "email": "{{OFFICE_8}}",
    "website": "{{OFFICE_9}}"
  },
  "address": {
    "careOf": "",
    "fullStreetAddress": "",
    "addressStreet": "",
    "addressStreet2": "",
    "city": "",
    "state": "",
    "zip": "",
    "zip4": ""
  },
  "mailingAddress": {
    "careOf": "",
    "fullStreetAddress": "",
    "addressStreet": "",
    "addressStreet2": "",
    "city": "",
    "state": "",
    "zip": "",
    "zip4": ""
  },
  "brokerInfo": {
    "brokerLicenseNum": "",
    "brokerAgentId": "{{BROKERID}}",
    "brokerCode": "{{BROKERSHORT}}",
    "designatedBroker": "",
    "brokerNrds": "",
    "contactMember": "",
    "franchiseNrds": "",
    "numBranches": ""
  }
}
```