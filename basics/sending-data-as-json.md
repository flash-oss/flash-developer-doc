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

You need to send us the JSON object with two properties - `"query"` and `"variables"`.

* Declare the `$input` variable in the QraphQL `"query"` string. The technology also requires you to declare the type of your input(s). See the `QueryInput` in the example below.
* Provide the `"variables"` object with the `"input"` property. The value of it must be a JSON object structured exactly as the `QueryInput` type.

{% hint style="warning" %}
Do NOT provide any other GraphQL types except the highest level input argument. Otherwise, you risk to experience production issues when we deploy schema changes.

If you construct the GraphQL query using a third party library/software then you risk to violate the above requrement. We recommend to not use any GraphQL libraries.
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

Here is a screenshot of how a typical mutation looks in the GraphQL Playground:

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
