---
description: Send money from your Flash Payments balance to Australian bank accounts
---

# Withdraw funds



To do a withdrawal (AUD only) you need to execute the `createWithdrawal` mutation as below.&#x20;

{% hint style="info" %}
Note you must have an AUD [balance](../balance.md) in your account to do a withdrawal.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createWithdrawal(
    input: {
      amount: 1000
      recipientId: "5ba89a6b35a2b327b81ffc3b",
      senderId: "5eaf71a1cb328c56f94f9375",
      acceptingInstructionInstitutionSenderId: "5eaf7159cb328c56f94f936d",
      acceptingMoneyInstitutionSenderId: "5eaf9e9b1c84a7678d3f7c6c"
      externalId: "12344321"
      idempotencyKey: "12344321"
    }
  ) {
    success
    code
    message
    withdrawal {
      id
      status
      amount
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createWithdrawal": {
      "success": true,
      "code": "OK",
      "message": "Scheduled for immediate execution",
      "withdrawal": {
        "id": "60711af8c078ba061f623531",
        "status": "PENDING",
        "amount": 1000
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Recipient - `recipientId`

You should [pre-create recipients](../recipients/#create-a-recipient) and provide us their ID. The recipient's account must be either `BSB` or `PAYID`.

### Sender - `senderId`

You should [pre-create senders](../senders.md#create-a-sender) and provide us their ID.

{% hint style="warning" %}
We are legally obliged to collect the actual sender and beneficiary details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

Please, send us the final funds sender and recipient. If sending to self then please provide your own details. See the schema in [Playground](https://api.flash-payments.com/) for other recipient details options.
{% endhint %}

### Sender Institution - `acceptingInstructionInstitutionSenderId`

This optional element is a reference to a [`sender`](../senders.md) object to give the information about the party who this withdrawal ultimately came from. This is normally the financial institution that accepts an **instruction** from a customer to transfer money electronically to a beneficiary institution so that it reaches the intended recipient. This is not the sender. If there is no other institution who has instructed this withdrawal, leave this blank and your own details will be used for compliance and auditing purposes.

### Accepting Institution - `acceptingMoneyInstitutionSenderId`

This optional element is a reference to a [`sender`](../senders.md) object to give the information about the party who this withdrawal ultimately came from. This is normally the financial institution that accepts **money** from a customer to transfer money electronically to a beneficiary institution so that it reaches the intended recipient. These details are needed for AML/KYC obligations here in Australia. This is not the sender. If there is no other institution who has accepted money that will result with this withdrawal, leave this blank and your own details will be used for compliance and auditing purposes.

### Callback (aka [Webhook](../webhooks/adhoc-webhooks.md)) URI

We recommend against continuous polling for withdrawal status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a withdrawal. These callbacks will usually occur soon (within several seconds) after the initial create withdrawal call - but may be delayed in some cases. The example JSON payloads can be found on the [Webhooks page](../webhooks/#example-payloads).

{% hint style="danger" %}
#### Security note

The callback (aka [webhook](../webhooks/adhoc-webhooks.md)) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector. See [Webhooks](../webhooks/adhoc-webhooks.md) page to secure your data properly.
{% endhint %}
