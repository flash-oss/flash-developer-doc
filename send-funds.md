# Send funds

To send funds you need to execute the `createPayment` mutation.

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createPayment(
    input: {
      paymentType: SEND_FUNDS
      fromCurrency: AUD
      toCurrency: XRP
      size: 1000
      currency: AUD
      reason: BUSINESS
      recipient: {
        firstName: "Sherlock"
        lastName: "Holmes"
        email: "detective@example.com"
        address: {
          street: "221b Baker St"
          suburb: "SYDNEY"
          state: "NSW"
          postcode: "2000"
          country: AU
        }
        rippleAddress: "r14cZWbgRsDwSVxxbG8r2VhVmz8D9zkNb6z"
      }
      externalReference: "my ref 221b"
      callbackUri: "https://example.com/flashfx?signature=ASecretPerPaymentKey"
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

### Callback \(aka Webhook\) URI

The optional `callbackUri` will be invoked few times while payment is being processed. The example JSON payloads are below.

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

{% hint style="danger" %}
#### Security note

The callback \(aka webhook\) endpoint URI can be invoked by anyone in the internet. Thus opening up a potential attack vector.
{% endhint %}

We highly recommend everyone to generate and add `?signature=ASecretPerPaymentKey` query to your `callbackUri` to make make sure it's FlashFX calling your webhook endpoint. To avoid storing the signatures in the database it can be a [JWT](https://jwt.io/) or any other securely verifiable string. For example:

```text
https://my-webhooks.example.com/flashfx?signature=oZaDlmfXbdXSKCnuWrvos2ImVBFX2Ru5
```

