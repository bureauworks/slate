# Terminology

Methods that provide support data for `Terminology` requests.

<!---




    Approval workflows




-->

## Term creation

Request with this `model` in the body of the request, where `language` is code from the Language API, and `glossaryIds` are IDs from the Glossary API.
Possible values for `part` are: `Noun`, `Pronoun`, `Adjective`, `Determiner`, `Verb`, `Adverb`, `Preposition`, `Conjunction` and `Interjection`. 
Possible values for `gender` are: `Masculine`, `Feminine`, `Neuter` and `NA`. Possible values for `termType` are: `Full` and `Acronym`

```json
{"terms":
   [
       {
            "text":"First term",
            "language":"en_us",
            "definition":"Term definition",
            "usage":"Usage for first term",
            "productLine":"",
            "domain":"",
            "subdomain":"",
            "imageUrl":"",
            "freeText":"",
            "note":"Note about this term",
            "part":"Noun",
            "gender":"Masculine",
            "forbidden":true,
            "exactMatch":true,
            "caseSensitive":true,
            "writable":false,
            "rtl":true,
            "internal":true,
            "status":"New",
            "termType":"Full",
            "dnt":"true"
        }
    ],
    "glossaryIds":[8,3475]
}
```

```shell
curl "https://bureau.works/api/term-arbitration/bulk-create"
  -H "X-AUTH-TOKEN: <TOKEN>"
  -X POST -d '<MODEL>'
```

> Response `200` `application/json`

```json
{
    "totalTermsRequested":0,
    "termCreationRequests":
    [
        {
            "id":1318,
            "status":"REQUESTED",
            "type":"Create",
            "conceptText":"First term",
            "totalOfterms":29,
            "targetGlossaries":[8,3475]
        }
    ]
}
```

This will create an approval workflow for newly added terms to the selected glossaries.
You must use the value in `code` fields to populate requests with source or target languages.

### HTTP Request

`POST https://bureau.works/api/term-arbitration/bulk-create`

<!---




    Request Update




-->

## Request Update

This method returns an DTO if the update is successful. The response body contains information about the term, such as its ID and language. During the update process, a request is send to the cat tool, which further delays a response.

```json
{
    "targetConcept":408397,
    "languages":["es_es"]
}
```

```shell
curl "https://bureau.works/api/term-arbitration/request-update"
  -H "X-AUTH-TOKEN: <TOKEN>"
  -X POST -d '<MODEL>'
```

> Response `200` with a JSON formatted like this:

```json
{
    "id":1321,
    "status":"REQUESTED",
    "type":"Update",
    "conceptText":"term2",
    "terms":
    [
        {
           "id":6729,
           "text":"text to translate",
           "language":"es_es"
        }
    ],
    "targetGlossaries":[3475]
}
```


### HTTP Request

`POST https://bureau.works/api/term-arbitration/request-update`

<!---




    Search Terms




-->

## Search Terms

This method returns all Terms related to the arguments in query parameter. These terms are grouped by Concept.


```shell
curl "https://bureau.works/api/glossary/term/search"
  -H "X-AUTH-TOKEN: <TOKEN>"
  -X POST -d '<MODEL>'
```

> Response `200` with a JSON formatted like this:

```json
{
    "content":
    [
        {
            "id":994004,
            "memsourceId":"rqJM8flS0Kt5aA9MXoxebVdl0",
            "text":"term2",
            "language":"en_us",
            "concept":
                {
                    "id":408397,
                    "memsourceId":"xSpdDxpK06y40xi08H503VSK3",
                    "writable":null
                },
            "glossary":
                {
                    "id":3475,
                    "memsourceTermBaseId":"285443",
                    "name":"TB Test - BW",
                    "dateCreated":1571071012785,
                    "createdBy":12345,
                    "client":null,
                    "domain":null,
                    "subDomain":null,
                    "note":null,
                    "langs":null
                },
            "memsourceClientId":"3926",
            "forbidden":false,
            "exactMatch":false,
            "status":"Under_Review",
            "memsourceStatus":"Approved",
            "caseSensitive":false,
            "createdAt":1571071012785,
            "lastModifiedAt":1571071093700,
            "rtl":false,
            "writable":false,
            "termArbitrationStatus":"IN_PROGRESS",
            "isRequestedReview":false,
            "requestReviewBy":null,
            "requestReviewAt":null,
            "commentsAboutReview":null,
            "usage":"",
            "definition":"",
            "dnt":null,
            "gender":"NA",
            "termType":"Full",
            "internal":false,
            "part":"Noun",
            "note":"",
            "termsLinked":[]
        }
    ],
    "pageable":
        {
            "sort":
            {  
                "sorted":true,
                "unsorted":false,
                "empty":false
            },
            "offset":0,
            "pageSize":20,
            "pageNumber":0,
            "paged":true,
            "unpaged":false
        },
        "totalElements":1,
        "totalPages":1,
        "last":true,
        "size":20,
        "first":true,
        "sort":
        {
            "sorted":true,
            "unsorted":false,
            "empty":false
        },
        "numberOfElements":1,
        "number":0,
        "empty":false
}
```

### HTTP Request

`GET https://bureau.works/api/glossary/term/search`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
caseSensitive | NO | Possible values are: true and false
exactMatch | NO | Possible values are: true and false
forbidden | NO | Possible values are: true and false
glossaryIds | NO | IDs from the Glossary API
language | NO | Possible values: A only Code from the Language API or 'All' string
matchType | YES | Possible values are: `Contains`, `StartsWith`, `EndsWith` and `ExactMatch`
page | YES | Number of page
pageSize | YES | Size of page
query | NO | String used to search a term 
rtl | NO | Possible values are: true and false
status | NO | Possible value: `Under_Review`
status | NO | Possible value: `Approved`
status | NO | Possible value: `Published`
writable | NO | Possible values are: true and false
