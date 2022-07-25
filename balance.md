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

Normally you could simulate the top up of your balance in the development environment by making a test deposit directly from Deposit menue on the FlashConnect website. However, this feature is currently not available due to temporarily limitations from our side.&#x20;

![](<.gitbook/assets/image (1).png>)

Therefore, when you need to top up your development account balance please reach out to our FlashFX Dev Support team either by email dev@flash-fx.com.au or in our shared Slack chanel and we will send you a number of test deposits that you need during our business hours.

