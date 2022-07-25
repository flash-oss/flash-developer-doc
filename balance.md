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

Top up your development account balance

When you need to top up your development account balance please reach out to our FlashFX Dev Support team either by email dev@flash-fx.com.au or in our shared Slack chat and we will be happy to help you during our business hours.

In the future we plan to enable your balance increase through test deposit simulation from our FlashConnect back-office website.&#x20;

