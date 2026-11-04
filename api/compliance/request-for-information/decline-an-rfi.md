---
description: When you cannot provide the requested information
---

# Decline an RFI

Use the `declineRfi` mutation when you are unable to provide the requested information — the API equivalent of "Unable to comply" on the secure form.

Declining:

* marks the RFI as `CLOSED` — an [`rfi_closed`](../../basics/webhooks/#rfi_closed) webhook is dispatched;
* cancels any linked deposits, withdrawals, or payments **still under review** at the time of decline, with [reject code](../../moving-funds/payouts/rejection-codes.md) `CANCELLATION_REQUESTED_BY_PARTICIPANT` ;
* leaves anything already confirmed, cancelled, or refunded as-is.

{% hint style="warning" %}
Only `PENDING` RFIs can be declined, and the action is **irreversible** — once declined, no further answers are accepted and the cancelled transactions can not be reinstated.
{% endhint %}

### Decline an RFI by ID

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      rfiId: "61f3a2c8d1e9b7a4c5d6e7f8",
    },
  },
  query: `
mutation ($input: DeclineRfiInput!) {
  declineRfi(input: $input) {
    success
    code
    message
    rfi {
      id
      status
      statusMessage
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: DeclineRfiInput!) {
  declineRfi(input: $input) {
    success
    code
    message
    rfi {
      id
      status
      statusMessage
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "rfiId": "61f3a2c8d1e9b7a4c5d6e7f8"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "declineRfi": {
      "success": true,
      "code": "MARKED",
      "message": "RFI marked as unanswered",
      "rfi": {
        "id": "61f3a2c8d1e9b7a4c5d6e7f8",
        "status": "CLOSED",
        "statusMessage": "You specified that you are unable to comply. Relevant transfers can be cancelled."
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

If the RFI is no longer `PENDING`, the mutation returns `success: false` with code `INVALID_STATUS` and the unchanged RFI — see [RFI response codes](rfi-response-codes.md).
