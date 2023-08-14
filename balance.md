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

The `cleared` is the **Primary** balance you see on the FlashConnect website.

The `pending` balance is typically not shown on the FlashConnect interface because it's rather short lived. If your **Pending** balance is not 0 then you can see it right next to the **Primary** balance.

### Top up your development account balance

If you need to increase your **development** account balance please sign in to the **FlashConnect** website, open **Deposits** page, and click **Send Test Deposit**.



