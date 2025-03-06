# Send funds

To make a payment from AUD to another currency you need to execute the `createPayment` mutation as below.&#x20;

{% hint style="info" %}
Note that you must have enough AUD balance to make an outbound AUD payment.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createPayment(
    input: {
      fromCurrency: AUD
      toCurrency: USD
      size: 1000
      currency: AUD
      reason: BUSINESS
      sourceOfFunds: BUSINESS_FUNDS
      externalReference: "my ref 221b"
      senderId: "6092360ae40e2cfb52f85be1"
      recipientId: "5ba89a6b35a2b327b81ffc3b"
      externalId: "12344321"
      idempotencyKey: "12344321"
    }
  ) {
    success
    code
    message
    payment {
      id
      status
      size
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createPayment": {
      "success": true,
      "code": "SUCCESS",
      "message": "Scheduled for immediate execution",
      "payment": {
        "id": "60711af8c078ba061f623531",
        "status": "OPEN",
        "size": 1000
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Recipient - recipient`Id`

You should [pre-create recipients](../recipients/#create-a-recipient) and send us their ID.

{% hint style="warning" %}
We are legally obliged to collect the actual beneficiary details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

Please, send us the final funds recipient. If sending to self then please provide your own details. See the DOCS in [Playground](https://api.uat.flash-payments.com.au/) for other recipient details options.
{% endhint %}

### Sender - `senderId` or `subClientId` <a href="#sender-senderid-or-subclientid" id="sender-senderid-or-subclientid"></a>

You should [pre-create senders](https://developer.flash-payments.com/senders#create-a-sender) and provide us with their ID. Alternatively, if your account is configured to make FX payments **on behalf of** your [sub-clients](https://developer.flash-payments.com/sub-clients), you may provide us with the sub-client ID, and the payments created will be linked to that sub-client. If the `senderId` is not provided, the `subClientId` will be used as the sender and will be reported to the government.

To use `subClientId` as the sender for your payments, please execute the `createPayment` mutation as below.

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createPayment(
    input: {
      fromCurrency: AUD
      toCurrency: USD
      size: 1000
      currency: AUD
      reason: BUSINESS
      sourceOfFunds: BUSINESS_FUNDS
      externalReference: "my ref 221c"
      subClientId: "6092360bf40f2dgc52f85cf1"
      recipientId: "5ba89a6b35a2b327b81ffc3b"
      externalId: "12344321"
      idempotencyKey: "12344321"
    }
  ) {
    success
    code
    message
    payment {
      id
      status
      size
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createPayment": {
      "success": true,
      "code": "SUCCESS",
      "message": "Scheduled for immediate execution",
      "payment": {
        "id": "60711bg8d078cb061g623531",
        "status": "OPEN",
        "size": 1000
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Callback (aka [Webhook](../webhooks/adhoc-webhooks.md)) URI

We recommend against continuous polling for payment status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a payment. These callbacks will usually occur soon (within several seconds) after the initial create payment call - but may be delayed in some cases. The example JSON payloads can be found on the [Webhooks page](../webhooks/#example-payloads).

Please note that `toAmount` (or `fromAmount`) and other fluctuating payment properties can change during payment execution.

{% hint style="danger" %}
#### Security note

The callback (aka [webhook](../webhooks/adhoc-webhooks.md)) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector. See [Webhooks](../webhooks/adhoc-webhooks.md) page to secure your data properly.
{% endhint %}
