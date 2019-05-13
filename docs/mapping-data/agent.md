---
title: Agent
sidebar_title: Agent
permalink: /docs/mapping-data/agent
---
Agent data mapping
```
{
  "_modified": {{#date}}{{TIMESTAMP}}{{/date}},
  "_recip": false,
  "_recipMLS": "",
  "_subdomain": "{{fetchSubdomain LICENSE MEMBER_2 MEMBER_17}}",
  "_suggestPriority": {{fetchSuggestPriority LICENSE MEMBER_2 MEMBER_17}},
  "mls" :{
    "isActive": {{#bool}}{{STATUS}}{{/bool}},
    "id": "{{#lowercase}}{{MEMBER_17}}{{/lowercase}}",
    "licNum": "{{LICENSE}}",
    "licExpirationDate": null,
    "licType": "",
    "transferDate": null,
    "realtorBoard": "{{MEMBER_20}}"
  },
  "office": {
    "id": {{#if MEMBER_1}}"example-{{MEMBER_1}}"{{else}}null{{/if}},
    "number": "{{OFFICESHORT}}",
    "mlsId": "{{MEMBER_1}}",
    "name": ""
  },
  "name": {
    "first": "{{MEMBER_3}}",
    "middle": "{{MEMBER_18}}",
    "last": "{{MEMBER_4}}",
    "full": "{{MEMBER_19}}"
  },
  "specifics": {
    "designation": "",
    "memberNum": "{{MEMBER_17}}",
    "nrdsNum": "{{#lowercase}}{{MEMBER_2}}{{/lowercase}}",
    "languages": [{{#stringToList}}{{LANGUAGE}}{{/stringToList}}],
    "specialties": "",
    "zipCodeServed": "",
    "birthDay": null,
    "title": "",
    "gender": "",
    "generation": "",
    "bio": "",
    "agentType": "",
    "agentRole": "",
    "agentStatus": "",
    "agentStatusDate": null,
    "lastModifiedDate": {{#date}}{{TIMESTAMP}}{{/date}}
  },
  "contact":{
    "email": {{#if MEMBER_10}}"{{MEMBER_10}}"{{else}}null{{/if}},
    "website": {{#if MEMBER_11}}"{{MEMBER_11}}"{{else}}null{{/if}},
    "contactPhoneLabel": "",
    "phoneExtension": null,
    "fax": {{#if MEMBER_8 }}"{{getPhone MEMBER_8}}"{{else}}null{{/if}},
    "officePhone": {{#if MEMBER_5 }}"{{getPhone MEMBER_5}}"{{else}}null{{/if}},
    "homePhone": {{#if MEMBER_7 }}"{{getPhone MEMBER_7}}"{{else}}null{{/if}},
    "mobilePhone": {{#if MEMBER_6 }}"{{getPhone MEMBER_6}}"{{else}}null{{/if}},
    "phones": [
      {{#if MEMBER_21 }}
      {
        "phoneComplete": "{{getPhone MEMBER_21}}",
        "phone": "{{MEMBER_21}}",
        "areaCode": "",
        "type": "Office",
        "additionalType": ""
      }
      {{/if}}
    ]
  },
  "address": {
    "fullStreetAddress": "{{MEMBER_12}}",
    "addressStreet": "",
    "city": "{{MEMBER_14}}",
    "state": "{{MEMBER_15}}",
    "zip": "{{MEMBER_16}}"
  },
  "mailingAddress": {
    "fullStreetAddress": "",
    "addressStreet": "",
    "addressStreet2": "",
    "city": "",
    "state": "",
    "zip": ""
  },
  "images": {{fetchImages "agent" true "images"}}
}
```
