# Send funds

To make a payment from AUD to another currency you need to execute the `createPayment` mutation as below. 

{% hint style="info" %}
Note you must have an AUD balance in your account to make an outbound AUD payment.
{% endhint %}

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

The optional `callbackUri` will be invoked several times during the processing of a  payment.  These callbacks will usually occur soon \(within several seconds\)  after the initial create payment call - but may be delayed in some cases.  The example JSON payloads are below.

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

We recommend API clients generate and add `?signature=ASecretPerPaymentKey` query to your `callbackUri` to make make sure it's FlashFX calling your webhook endpoint. To avoid storing the signatures in the database it can be a [JWT](https://jwt.io/) or any other securely verifiable string. For example:

```text
https://my-webhooks.example.com/flashfx?signature=oZaDlmfXbdXSKCnuWrvos2ImVBFX2Ru5
```

You can also use the callback as a trigger to retrieve the latest payment details via the Get Payment api call.

