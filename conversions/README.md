---
description: Convert between your multi currency balances
---

# Conversions

To convert between, say, `AUD` and `EUR` you should:

1. Request a tradeable quote with applicability: CONVERSION.
2. Retrieve the quote ID from the response.
3. Reference the quote ID when creating the conversion.

{% hint style="warning" %}
We suggest always sending your queries and related data separately using the "QUERY VARIABLES" tab in the [API playground](https://api.uat.flash-payments.com.au/) or programmatically by [submitting the variables as JSON](https://developer.flash-payments.com/basics/sending-data-as-json).
{% endhint %}

Get a tradable quote ID:

{% tabs %}
{% tab title="Query" %}
```graphql
query($input: QuoteInput!) {
  quote(input: $input) {
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

{% tab title="Query Variables" %}
```javascript
{
  "input": { 
     "fromCurrency": "AUD", 
     "toCurrency": "EUR", 
     "size": "10000", 
     "currency": "AUD",
     "tradeable": true,
     "applicability": "CONVERSION"
 }
}
```
{% endtab %}

{% tab title="Embedded Variables" %}
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
      "id": "685369cf85f7cb51fb4ae2eb",
      "bid": 0.55663,
      "ask": 0.57153,
      "symbol": "AUDEUR",
      "timestamp": "2025-06-19T01:37:19.418Z",
      "inverted": false,
      "expireAt": "2025-06-19T01:39:19.419Z"
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
 mutation($input: ConversionInput!) {
  createConversion(input: $input) {
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
```
{% endtab %}

{% tab title="Query Variables" %}
```javascript
 {
  "input": { 
      "note": "for major client",
      "externalId": "561402",
      "quoteId": "6711ec0e950ae23de886ebe5",
      "callbackUri": "https://example.com/my-webhook/endpoint/"
 }
}
```
{% endtab %}

{% tab title="Embedded Variables" %}
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
