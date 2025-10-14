---
description: How to avoid and resolve HTTP 429 Too Many Requests
---

# Rate limiting

## Limits

Various limits apply depending on the type of HTTP request or GraphQL query being executed, as well as the number of such requests. The rate-limiting mechanism is quite dynamic and is based on the IP address(es), error rates, HTTP headers, and the GraphQL queries.

When one of the numerous limits exceeds the API typically responds with

1. the HTTP status `429`&#x20;
2.  the HTTP header:\


    ```
    Retry-After: 59
    ```
3.  and a JSON body:\


    ```json
    {
      success: false,
      code: "TOO_MANY_REQUESTS",
      message: "Retry after 1 min",
      retrySecs: 59
    }
    ```

## Common cases

Here are the common situations when you might get yourself temporarily blocked:

* When your requests produce numerous errors, e.g., malformed header, JSON body, or a GraphQL query.
* When you send too many requests without a valid `Authorization` header.
* When you send too requests within a very short period.
  * Most often reason - when you collect reference prices/quotes using the `quote` query. To avoid the issue you must [send multiple GraphQL queries within a single HTTP request](../fx/quote.md#multiple-reference-indicative-quotes-with-a-single-http-request).

## How to reset if you see HTTP 429 responses

Please contact our support team via the established communication channels. We will manually reset the counters for you.
