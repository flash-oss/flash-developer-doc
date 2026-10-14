---
description: >-
  How a deposit addressed to a sub-client's PayID shows up in the API and in
  webhooks
---

# Deposits to a PayID

Once you have registered a PayID on a sub-client’s Virtual Account Number (VAN), your customer can pay into their VAN by that alias instead of the Australian BSB and account number.

Before paying, their bank shows them the account name registered for the PayID, which is the sub-client’s name as registered with Flash. The transaction arrives as an ordinary deposit on the VAN, booked on your master account balance and linked to the sub-client, exactly like a deposit to the BSB and account number: the same statuses, the same webhooks and the same queries.

#### Which PayID a deposit was addressed to

The deposit's `recipient` records it: `accountIdType` is `PAYID` and `payid` holds the address, next to the `bsb` and `accountNo` of the virtual account it was paid into. For a deposit paid to the BSB and account number, `accountIdType` is `BSB` and `payid` is `null`. To reconcile per PayID, match on `recipient.payid`.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "68e5c1a9d2f4b07a3c9e1d55",
  },
  query: `
query ($input: ID) {
  deposit(id: $input) {
    id amount currency status externalReference createdAt
    subClient {
      id fullName
    }
    recipient {
      accountIdType payid bsb accountNo accountName
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ID) {
  deposit(id: $input) {
    id
    amount
    currency
    status
    externalReference
    createdAt
    subClient {
      id
      fullName
    }
    recipient {
      accountIdType
      payid
      bsb
      accountNo
      accountName
    }
    # more fields available, see API schema
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": "68e5c1a9d2f4b07a3c9e1d55"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "deposit": {
      "id": "68e5c1a9d2f4b07a3c9e1d55",
      "amount": 250,
      "currency": "AUD",
      "status": "CONFIRMED",
      "externalReference": "INV-10422",
      "createdAt": "2026-10-08T04:17:52.310Z",
      "subClient": {
        "id": "606d28675a2d931bc925fec2",
        "fullName": "ACME Corp"
      },
      "recipient": {
        "accountIdType": "PAYID",
        "payid": "acme-corp@example.com",
        "bsb": "802919",
        "accountNo": "1066419",
        "accountName": "ACME Corp"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Webhooks

The deposit webhooks carry the PayID too: the `recipient` object of every `deposit_*` payload has `payid` next to `accountName`, `bsb` and `accountNo`. The field is absent for deposits paid to the BSB and account number.

```javascript
// excerpt of a deposit_cleared payload
{
  "id": "68e5c1a9d2f4b07a3c9e1d55",
  "amount": 250,
  "currency": "AUD",
  "externalReference": "INV-10422",
  "recipient": {
    "accountName": "ACME Corp",
    "accountNo": "1066419",
    "bsb": "802919",
    "payid": "acme-corp@example.com"
  },
  // sender, subClient and the other fields as for any deposit
}
```
