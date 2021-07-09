# Send funds

To make a payment from AUD to another currency you need to execute the `createPayment` mutation as below. 

{% hint style="info" %}
Note you must have an AUD [balance](../balance.md) in your account to make an outbound AUD payment.
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

### Recipient

You should [pre-create recipients](../recipients/#create-a-recipient) and send us their ID.

{% hint style="warning" %}
We are legally obliged to collect the actual beneficiary details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

Please, send us the final funds recipient. If sending to self then please provide your own details. See the DOCS in [Playground](https://api.flash-fx.com/) for other recipient details options.
{% endhint %}

### Callback \(aka [Webhook](../webhooks/adhoc-webhooks.md)\) URI

We recommend against continuous polling for payment status changes. Instead, please use `callbackUri`.

The optional `callbackUri` will be invoked several times during the processing of a payment. These callbacks will usually occur soon \(within several seconds\) after the initial create payment call - but may be delayed in some cases. The example JSON payloads are below.

{% tabs %}
{% tab title="" %}
```javascript
{
  "event": "payment_complete",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "USD",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="currency\_converted" %}
```javascript
{
  "event": "currency_converted",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "USD",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="payment\_complete" %}
```javascript
{
  "event": "payment_complete",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "USD",
  "externalId": "12344321"
}
```
{% endtab %}
{% endtabs %}

Please note that `toAmount` \(or `fromAmount`\) and other fluctuating payment properties can change during payment execution.

{% hint style="danger" %}
#### Security note

The callback \(aka [webhook](../webhooks/adhoc-webhooks.md)\) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector. See [Webhooks](../webhooks/adhoc-webhooks.md) page to secure your data properly.
{% endhint %}

