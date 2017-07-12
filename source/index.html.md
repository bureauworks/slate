---
title: Bureau Works API

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

This API implements methods for creation and management of translation projects with [Bureau Works](https://bureau.works).






# Authentication

> Auth example, uses a JSON model in the request body:

```json
{
    "username": "email",
    "password": "password",
    "token": "user_api_token"
}
```

```shell
curl "https://bureau.works/api/pub/v1/login"
  -H "Content-Type: application/json"
  -X POST -d '<MODEL>'
```

> Response `200` `X-AUTH-TOKEN: <TOKEN>`

Use this method to get the credentials necessary to access the API endpoints. 

If authentication is successful, returns a header `X-AUTH-TOKEN` that must be used in all subsequent API calls:

`X-AUTH-TOKEN: {token}`

### HTTP Request

`POST https://bureau.works/api/pub/v1/login`

<aside class="notice">
You must replace <code>token</code> with your personal API key.
</aside>





# Locale

## Languages

```shell
curl "https://bureau.works/api/pub/v1/language"
  -H "X-AUTH-TOKEN: <TOKEN>"
```

> Response `200` `application/json`

```json
[
  {
      "id": 2,
      "name": "Abkhaz",
      "code": "ab"
  },
  {
      "id": 1,
      "name": "Afar",
      "code": "aa"
  },
  {
      "id": 3,
      "name": "Afrikaans",
      "code": "af"
  }
]
```

These are all available languages in Bureau Works. If your project demands a translation in a language that is not
listed by this API, please contact us to resolve the issue.

You must use the value in `code` fields to populate requests with source or target languages.

### HTTP Request

`GET https://bureau.works/api/pub/v1/language`

## Timezones

```shell
curl "https://bureau.works/api/pub/v1/timezone"
  -H "X-AUTH-TOKEN: <TOKEN>"
```

> Response `200` with a JSON formatted like this:

```json
[
    {
        "key": "AET",
        "value": "(GMT+10:00) AET"
    },
    {
        "key": "Antarctica/DumontDUrville",
        "value": "(GMT+10:00) Antarctica/DumontDUrville"
    },
    {
        "key": "Asia/Ust-Nera",
        "value": "(GMT+10:00) Asia/Ust-Nera"
    },
    {
        "key": "Asia/Vladivostok",
        "value": "(GMT+10:00) Asia/Vladivostok"
    },
    {
        "key": "Australia/ACT",
        "value": "(GMT+10:00) Australia/ACT"
    }
]
```

Timezones are used according to a client's or person's location. When creating a project, you must specify a timezone in order to correctly calculate creation date, and also due and delivery dates.

In your requests, you may use the `key` to populate `timezone` fields.

### HTTP Request

`GET https://bureau.works/api/pub/v1/timezone`




# Project

API to manage projects.

## Create Project

> Request with this `model`, where `sourceLanguage` and `targetLanguages` are codes from the Language API, and `services` are IDs from the Service API.

```json
{
    "reference": "API TEST",
    "sourceLanguage": "en_us",
    "targetLanguages": [
        "pt_br",
        "es"
    ],
    "services": [
        1
    ],
    "notes": "some notes here",
    "desiredDeliveryDate": 0
}
```

```shell
curl "https://bureau.works/api/pub/v1/project"
  -H "X-AUTH-TOKEN: <TOKEN>"
  -X POST -d '<MODEL>'
```

> Response `200` `application/json`

```json
{
    "id": 898,
    "client": {
        "id": 120,
        "name": "Bureau Translations",
        "invoiceInfo": null
    },
    "currency": "USD",
    "name": "B-17193-58704",
    "reference": "API TEST HC",
    "sourceLanguage": "en_us",
    "quoteDueDate": null,
    "creationDate": 1499890704612,
    "status": "IN_PREPARATION",
    "grandTotal": null,
    "delivered": false,
    "targetLanguages": [
        "pt_br",
        "es"
    ],
    "tags": [],
    "items": [
        {
            "id": 1171,
            "serviceId": 1,
            "serviceName": "Translation",
            "originalFiles": [],
            "filesDeliveredByTranslators": [],
            "filesDeliveredByManagers": [],
            "words": 0,
            "subtotal": 0,
            "savings": 0,
            "grandTotal": 0
        }
    ]
}
```

### HTTP Request

`POST https://bureau.works/api/pub/v1/project`


## Get Projects by Status

```shell
curl "https://bureau.works/api/pub/v1/project?status=PENDING"
  -H "X-AUTH-TOKEN: <TOKEN>"
```

> Response `200` `application/json`

```json
[
  {
      "id": 897,
      "client": {
          "id": 120,
          "name": "Bureau Translations",
          "invoiceInfo": null
      },
      "currency": "USD",
      "name": "B-17193-42869",
      "reference": "Test project 003",
      "sourceLanguage": "pt_br",
      "quoteDueDate": null,
      "creationDate": 1499874869268,
      "status": "PENDING",
      "grandTotal": 1371.9940000000001,
      "delivered": false,
      "targetLanguages": [
          "en_us",
          "es"
      ],
      "tags": [],
      "items": [
          {
              "id": 1170,
              "serviceId": 1,
              "serviceName": "Translation",
              "originalFiles": [
                  "508word2010.docx"
              ],
              "filesDeliveredByTranslators": [],
              "filesDeliveredByManagers": [],
              "words": 10876,
              "subtotal": 1413.88,
              "savings": 41.886,
              "grandTotal": 1371.994
          }
      ]
  }
]
```

Gets a list of projects, by passing at least one `status` as parameter. You can provide more than one status, but at least one.
An example request would be, which would list all pending AND approved projects for your organization in the system:

`GET /api/pub/v1/project?status=PENDING&status=APPROVED`

### HTTP Request
`GET /api/pub/v1/project`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
status | YES | A status to filter the request. Valid statuses are `IN_PREPARATION`, `PENDING`, `APPROVED`, `CANCELLED` and `INVOICED`
