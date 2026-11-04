---
description: Return the funds back to the original sender
---

# Refund deposits

#### Refund deposit by ID

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "6053d4e0e3bc655e0598a742",
    input: {
      amount: 10,
      reason: "sent by mistake",
    },
  }, 
  query: `
mutation ($id: ID!, $input: RefundDepositInput!) {
  refundDeposit(id: $id, input: $input) {
    success code message
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($id: ID!, $input: RefundDepositInput!) {
  refundDeposit(id: $id, input: $input) {
    success
    code
    message
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
  "id": "6053d4e0e3bc655e0598a742",
  "input": { 
    "amount": 10, 
    "reason": "sent by mistake"
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
