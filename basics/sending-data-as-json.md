---
description: >-
  Here is how to send your data to us as JSON instead of embedding it into the
  GraphQL queries.
---

# Sending data as JSON

Most of the documentation examples demonstrate how to send data by **embedding values** into the GraphQL queries.

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

To make it easier to work with complex queries in code, we support sending GraphQL operations as a JSON object with two properties: `"query"` and `"variables"`. Here is how you can do it:

* Declare the `$input` variable in the QraphQL `"query"` string. The technology also requires you to declare the type of your input(s). See the `QueryInput` in the example below.
* Provide the `"variables"` object with the `"input"` property. The value of it must be a JSON object structured exactly as the `QueryInput` type.

{% hint style="warning" %}
Only include the top-level input argument in your GraphQL queries. Including additional types may lead to compatibility issues when schema changes are deployed in production.

If you're using third-party tools or libraries to construct GraphQL queries, be aware that they may generate unsupported structures. We recommend avoiding GraphQL libraries to reduce this risk.
{% endhint %}

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

Here is a screenshot of how a typical mutation looks like in the API Playground:

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
