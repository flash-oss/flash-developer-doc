---
description: Quote the current bid and ask for a currency pair and size.
---

# Quote

Paste this query to the GraphQL Playground

{% tabs %}
{% tab title="Query" %}
```graphql
{
  quote(
    input: { fromCurrency: AUD, toCurrency: USD, size: 10000, currency: AUD }
  ) {
    bid
    ask
    symbol
    timestamp
    inverted
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "quote": {
      "bid": 0.61104,
      "ask": 0.62077,
      "symbol": "USDAUD",
      "timestamp": "2018-08-13T07:54:54.993Z",
      "inverted": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

