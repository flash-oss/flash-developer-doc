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
      callbackUri: "http://sherlock.proxy.beeceptor.com"
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

The `callbackUri` will be invoked couple of times while payment is being processed. The example JSON payloads are below.

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



