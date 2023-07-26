---
description: Return the funds back to the original sender
---

# Refund deposits

#### Refund deposit by ID

{% tabs %}
{% tab title="Request" %}
```graphql
mutation {
  refundDeposit(
    id: "6053d4e0e3bc655e0598a742"
    input: { amount: 10, reason: "sent by mistake" }
  ) {
    success
    code
    message
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "refundDeposit": {
      "success": true,
      "code": "REFUND_SENT",
      "message": "Refund initiated successfully"
    }
  }
}
```
{% endtab %}
{% endtabs %}
