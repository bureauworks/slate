# Errors

Bureau Works API Error Codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request went wrong due to a lack of information
401 | Unauthorized
403 | Forbidden
404 | Not Found
405 | Method Not Allowed
406 | Not Acceptable
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

> Some errors, like `Bad Request` come with JSON to inform you what went wrong, like this:

```json
{
    "status": 400,
    "exception": "java.lang.NullPointerException",
    "message": "NullPointerException: Client has no country associated",
    "data": null
}
```