# Quote

Quote the current bid and ask for a currency pair and size.

Paste this query to the GraphQL Playground

{% tabs %}
{% tab title="Query" %}
```graphql
{
  quote(
    input: { fromCurrency: AUD, toCurrency: XRP, size: 10000, currency: AUD }
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
      "bid": 0.41104,
      "ask": 0.42077,
      "symbol": "XRPAUD",
      "timestamp": "2018-08-13T07:54:54.993Z",
      "inverted": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

