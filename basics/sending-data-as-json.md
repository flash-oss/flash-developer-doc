---
description: >-
  Here is how to send your data to us as JSON instead of embedding it into the
  GraphQL queries.
---

# Sending data as JSON

Most of the documentation examples shows you how to send data by **embedding values** into the GraphQL queries.

```bash
echo '{
  "query":
    "{
       quote(input: {
         fromCurrency: AUD, toCurrency: USD, size: 9.9, currency: AUD
       })
       {
         bid ask symbol timestamp inverted
       }
     }"
}' | curl -X POST 'https://api.uat.flash-payments.com.au' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d @-
```

We understand that such long query strings would be difficult to construct with code. Here is how to send **query and data separately**.

Each GraphQL request to our API contains a JSON of minimum two properties: `query` and optional `variables`. You can (and should) declare parameters using the GraphQL dollar-notation and then reuse it in the query or mutation.

```bash
echo '{
  "query":
    "query ($input: QuoteInput!) {
       quote(input: $input) { bid ask symbol timestamp inverted }
     }",
  "variables": { 
    "input": { "fromCurrency": "AUD", "toCurrency": "USD", "size": 9.9, "currency": "AUD" }
  }
}' | curl -X POST 'https://api.uat.flash-payments.com.au' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d @-
```

Here is a screenshot of how a mutation looks in GraphQL Playground:

![](../.gitbook/assets/image.png)

Same request as cURL:

```bash
echo '{
  "query":
    "mutation ($input: RecipientInput!) {
      createRecipient(input: $input) { code message recipient { id } }
    }",
  "variables": {
    "input": {
      "firstName": "Test",
      "lastName": "Lastest",
      "currency": "PHP",
      "accountIdType": "PH_CASH",
      "mobile": "+63 9121231234",
      "phCashoutNetwork": "MLHUILLIER",
      "address": {
        "building": "12th Floor Centerpoint Building",
        "street": "Julia Vargas Avenue corner Garnet Street",
        "suburb": "Pasig",
        "state": "manila",
        "country": "PH",
        "postcode": "1605"
      }
    }
  }
}' | curl 'https://api.uat.flash-payments.com.au' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d @-
```
