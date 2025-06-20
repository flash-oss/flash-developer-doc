---
description: Convert between your multi-currency balances
---

# Convert funds

To convert between, say, `AUD` and `EUR` you should:

1. Request a tradeable quote with applicability: CONVERSION.
2. Retrieve the quote ID from the response.
3. Reference the quote ID when creating the conversion.

Get a tradable quote ID:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      fromCurrency: "AUD",
      toCurrency: "EUR",
      size: "10000",
      currency: "AUD",
      tradeable: true,
      applicability: "CONVERSION",
    },
  },
  query: `
query ($input: QuoteInput!) {
  quote(input: $input) {
    id bid ask symbol timestamp inverted expireAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
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

{% tab title="Variables" %}
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
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
        note: "for major client",
        externalId: "561402",
        quoteId: "6854dcffaa36ba8534d5f8e2",
        callbackUri: "https://example.com/my-webhook/endpoint/",
    }
  },
  query: `
mutation ($input: ConversionInput!) {
  createConversion(input: $input) {    
    success code message 
    conversion { 
      id fromCurrency toCurrency currencyPair fromAmount toAmount rate note 
      status statusMessage callbackUri externalId createdAt updatedAt 
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: ConversionInput!) {
  createConversion(input: $input) {
    success code message
    conversion {
      id fromCurrency toCurrency currencyPair fromAmount toAmount rate note
      status statusMessage callbackUri externalId createdAt updatedAt
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "note": "for major client",
    "externalId": "561402",
    "quoteId": "6854dcffaa36ba8534d5f8e2",
    "callbackUri": "https://example.com/my-webhook/endpoint/"
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
