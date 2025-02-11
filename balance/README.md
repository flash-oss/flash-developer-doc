---
description: Query your account balances
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

The `cleared` is the **Primary** balance you see on the Flash Connect website.

The `pending` balance is typically not shown on the Flash Connect interface because it's rather short lived. If your **Pending** balance is not 0 then you can see it right next to the **Primary** balance.

If you omit the `currencies` input argument then the query would return all the currency balances you have. Contact us to enable more than just the default `AUD` currency.

### Top up your UAT account balance

If you need to increase your **UAT** account balance please sign in to the **Flash Connect** website, open **Deposits** page, and click **Send Test Deposit**.



