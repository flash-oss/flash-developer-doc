---
description: Convert between your multi currency balances
---

# Conversions

To convert between, say, `AUD` and `EUR` you should:

1. request a tradeable quote with the `applicability: CONVERSION`,
2. use it whilst creating a conversion.

Get a quote ID:

{% tabs %}
{% tab title="Quote" %}
```graphql
{
  quote(
    input: { 
      fromCurrency: AUD
      toCurrency: EUR
      size: 10000
      currency: AUD
      tradeable: true
      applicability: CONVERSION
    }
  ) {
    id
    bid
    ask
    symbol
    timestamp
    inverted
    expireAt
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "quote": {
      "id": "6711ec0e950ae23de886ebe5",
      "bid": 0.61104,
      "ask": 0.62077,
      "symbol": "USDAUD",
      "timestamp": "2024-08-13T07:54:54.993Z",
      "inverted": true,
      "expireAt": "2024-08-13T07:55:54.993Z"
    }
  }
}
```
{% endtab %}
{% endtabs %}

Convert funds:

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createConversion(
    input: {
      note: "for major client"
      externalId: "561402"
      quoteId: "6711ec0e950ae23de886ebe5"
      callbackUri: "https://example.com/my-webhook/endpoint/"
    }
  ) {
    success
    code
    message
    conversion {
      id
      fromCurrency
      toCurrency
      currencyPair
      fromAmount
      toAmount
      rate
      note
      status
      statusMessage
      callbackUri
      externalId
      createdAt
      updatedAt
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "createConversion": {
      "success": true,
      "code": "CREATED",
      "message": "Conversion successfully created",
      "conversion": {
        "id": "6711f0a7b549a701ebeb02ac",
        "fromCurrency": "EUR",
        "toCurrency": "AUD",
        "currencyPair": "AUDEUR",
        "fromAmount": 101,
        "toAmount": 159.81,
        "rate": 0.63199,
        "note": "for major client",
        "status": "PENDING",
        "statusMessage": "Awaiting execution",
        "callbackUri": "https://example.com/my-webhook/endpoint/",
        "externalId": "561402",
        "createdAt": "2024-10-18T05:22:47.433Z",
        "updatedAt": "2024-10-18T05:22:47.433Z"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

###

