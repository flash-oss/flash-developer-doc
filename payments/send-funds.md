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
      toCurrency: XRP
      size: 1000
      currency: AUD
      reason: BUSINESS
      externalReference: "my ref 221b"
      callbackUri: "https://example.com/flashfx?signature=ASecretPerPaymentKey"
      recipientId: "5ba89a6b35a2b327b81ffc3b"
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

Please, send us the final funds recipient. If sending to self then please provide your own details. See the schema in [Playground](https://api.flash-fx.com/) for other recipient details options.
{% endhint %}

### Callback \(aka Webhook\) URI

The optional `callbackUri` will be invoked several times during the processing of a payment. These callbacks will usually occur soon \(within several seconds\) after the initial create payment call - but may be delayed in some cases. The example JSON payloads are below.

{% tabs %}
{% tab title="currency\_converted" %}
```javascript
{
  "event": "currency_converted",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "XRP"
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
  "toCurrency": "XRP"
}
```
{% endtab %}
{% endtabs %}

Please note that `toAmount` \(or `fromAmount`\) and other fluctuating payment properties can change during payment execution.

{% hint style="danger" %}
#### Security note

The callback \(aka webhook\) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector.
{% endhint %}

We recommend API clients generate and add `?signature=ASecretPerPaymentKey` query to your `callbackUri` to make make sure it's FlashFX calling your webhook endpoint. To avoid storing the signatures in the database it can be a [JWT](https://jwt.io/) or any other securely verifiable string. For example:

```text
https://my-webhooks.example.com/flashfx?signature=oZaDlmfXbdXSKCnuWrvos2ImVBFX2Ru5
```

You can also use the callback as a trigger to retrieve the latest payment details via the [payments query](query-payments.md).

