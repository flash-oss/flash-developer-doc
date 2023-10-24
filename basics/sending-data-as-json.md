---
description: >-
  Here is how to send your data to us as JSON instead of embedding it into the
  GraphQL queries.
---

# Sending data as JSON

Most of the documentation examples shows you how to send data by **embedding values** into the GraphQL queries.

```bash
curl -X POST 'https://api.flash-payments.com' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d '{
  "query":
    "{
       quote(input: {
         fromCurrency: AUD, toCurrency: USD, size: 9.9, currency: AUD
       })
       {
         bid ask symbol timestamp inverted
       }
     }"
}'
```

We understand that such long query strings would be difficult to construct with code. Here is how to send **query and data separately**.

Each GraphQL request to our API contains a JSON of minimum two properties: `query` and optional `variables`. You can (and should) declare parameters using the GraphQL dollar-notation and then reuse it in the query or mutation.

```bash
curl -X POST 'https://api.flash-payments.com' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d '{
  "query":
    "query ($input: QuoteInput!) {
       quote(input: $input) { bid ask symbol timestamp inverted }
     }"
  "variables": { 
    "input": { "fromCurrency": "AUD", "toCurrency": "USD", "size": 9.9, "currency": "AUD" }
  }
}'
```

Here is a screenshot of how a mutation looks in GraphQL Playground:

![](<../.gitbook/assets/image (1).png>)

Same request as cURL:

```bash
curl 'https://api.flash-payments.com/' \
  -H 'authorization: Bearer YOUR_TOKEN' \
  -H 'content-type: application/json' 
  -d $'{
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
}'
```
