---
description: This will send money from your FlashFX balance to bank account(s)
---

# Withdraw funds



To do a withdrawal \(AUD only\) you need to execute the `createWithdrawal` mutation as below. 

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
      recipientId: "5ba89a6b35a2b327b81ffc3b"
      externalId: "12344321"
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

### Recipient

You should [pre-create recipients](../recipients/#create-a-recipient) and send us their ID. The recipient's account must be either `BSB` or `PAYID`.

{% hint style="warning" %}
We are legally obliged to collect the actual beneficiary details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

Please, send us the final funds recipient. If sending to self then please provide your own details. See the schema in [Playground](https://api.flash-fx.com/) for other recipient details options.
{% endhint %}

### Callback \(aka [Webhook](../webhooks.md)\) URI

We recommend against continuous polling for withdrawal status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a withdrawal. These callbacks will usually occur soon \(within several seconds\) after the initial create withdrawal call - but may be delayed in some cases. The example JSON payloads are below.

{% tabs %}
{% tab title="withdrawal\_initiated" %}
```javascript
{
  "event": "withdrawal_initiated",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "currency": "AUD",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="withdrawal\_completed" %}
```javascript
{
  "event": "withdrawal_completed",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "currency": "AUD",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="withdrawal\_failed" %}
```javascript
{
  "event": "withdrawal_failed",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "currency": "AUD",
  "externalId": "12344321"
}
```
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
#### Security note

The callback \(aka [webhook](../webhooks.md)\) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector. See [Webhooks](../webhooks.md) page to secure your data properly.
{% endhint %}

