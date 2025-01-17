---
description: Understand every movement of your primary balance
---

# Statement

This query returns the same data as the Download CSV button on the Account Statement page of the [Flash Connect](https://connect.uat.flash-payments.com.au/).

The dates must be any ISO 8601 formatted dates.

{% tabs %}
{% tab title="Query" %}
```graphql
{
  statement(input: { fromDate: "2023-08-28T00:00:00+03:00" }) {
    success
    code
    message
    fromDate
    toDate
    rows {
      debit
      credit
      # and many other fields
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "statement": [
      {
        "success": true,
        "code": "GENERATED",
        "message": "Statement generated",
        "fromDate": "2023-08-27T21:00:00Z",
        "toDate": "2023-08-28T21:00:00Z",
        "rows": {
          [
            {
              "debit": 123.45,
              "credit": 0
            }
          ]
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

The response dates are aways UTC. Timezones are always taken into account.
