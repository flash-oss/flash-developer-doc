---
description: Query your account balance
---

# Balance

Paste this query to the GraphQL Playground

{% tabs %}
{% tab title="Query" %}
```graphql
{
  balances(currencies: AUD) {
    currency
    cleared
    pending
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "balances": [
      {
        "currency": "AUD",
        "cleared": 3860,
        "pending": 0
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}



