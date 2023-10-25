---
description: Send money from your Flash Payments balance to Australian bank accounts
---

# Withdraw funds



To do a withdrawal (AUD only) you need to execute the `createWithdrawal` mutation as below.&#x20;

{% hint style="info" %}
Note you must have an AUD [balance](../balance/) in your account to do a withdrawal.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createWithdrawal(
    input: {
      amount: 1000
      externalReference: "invoice #123"
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

### Payment reference - `externalReference`

Arbitrary text which will be seen in the ultimate recipient's bank statement. E.g. `"invoice #123"`. Will be eventually truncated to `18` ASCII chars if delivered via Australia's old (DE, Direct Entry) payment system. However, if you choose to use the real-time NPP network, then the maximum length is `280` chars.

### Recipient - `recipientId`

You should [pre-create recipients](../recipients/#create-a-recipient) and provide us their ID. The recipient's account must be either `BSB` or `PAYID`.

### Sender - `senderId`

You should [pre-create senders](../senders.md#create-a-sender) and provide us their ID.

{% hint style="warning" %}
We are legally obliged to collect the actual sender and beneficiary details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc. Please see`instructingInstitutionId` if it is an intermediate.

Please, send us the final funds sender and recipient. If sending to yourself then please provide your own details. See the schema in [Playground](https://api.flash-payments.com/) for other recipient details options.
{% endhint %}

### Instructing Institution - `instructingInstitutionId`

This optional field is a reference to an `Institution` object to give the information about the party who instructed you to do the withdrawal. This field is important to facilitate real-time settlement times within Australia. This is not the sender. If there is no other institution who has instructed this withdrawal, leave this blank and your own details will be used for compliance and auditing purposes.&#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Institution explanation</p></figcaption></figure>

#### Instructions to use

1. Create an Institution in Flash Connect
2. Copy an ID of the Institution and pass it into `instructingInstitutionId`

### Callback (aka [Webhook](../webhooks/adhoc-webhooks.md)) URI

We recommend against continuous polling for withdrawal status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a withdrawal. These callbacks will usually occur soon (within several seconds) after the initial create withdrawal call - but may be delayed in some cases. The example JSON payloads can be found on the [Webhooks page](../webhooks/#example-payloads).

{% hint style="danger" %}
#### Security note

The callback (aka [webhook](../webhooks/adhoc-webhooks.md)) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector. See [Webhooks](../webhooks/adhoc-webhooks.md) page to secure your data properly.
{% endhint %}
