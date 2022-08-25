---
description: How to setup web hooks for your integration
---

# Regular webhooks

### Event notifications with webhooks

FlashFX uses webhooks to notify your application when a transaction event happens in your account. This includes status update of your withdrawal, clearing of the incoming deposit, and more.

With webhooks, you can subscribe to the events of interest to trigger a subsequent action within your integration.

There are two steps to begin using webhooks. Building a custom endpoint on your server and registering it via the [FlashConnect](https://connect.flash-fx.com/) settings.

### Building webhook endpoint

Each time a transaction event happens, we will send a HTTP POST request to your endpoint(s) that are subscribed to that event. The payload with event data is in the JSON format and can be used immediately. Every webhook endpoint must acknowledge the request, and as we strongly advise, verify the webhook signature.

**Acknowledging request**

To acknowledge the receipt of the event, your endpoint must return 2xx HTTP status code. All other response codes, including 3xx codes, indicate that you did not receive the event.

If we do not receive 200 HTTP status code, we will make up to 3 more request attempts. There is 6, 60 and 600 seconds delay between each delivery attempt respectively. If all request attempts fail, we mark webhook request as failed and will no longer try to deliver it.

Each webhook request has a `flashfx-request-id` header with a string that uniquely identifies a webhook event. For example, if there are multiple delivery attempts for the webhook event, each request will hold the same `flashfx-request-id`. This can be used to check if the webhook event was already processed by your application.

We advise that you acknowledge webhook request as early as possible.

**Securing the request**

To ensure that webhook was sent by FlashFX and not a third party, we include the cryptographic signature in each request’s `flashfx-signature` header.

Before you can verify a signature, you need to retrieve your endpoint’s secret from webhook settings in the [FlashConnect](https://connect.flash-fx.com/). Each registered endpoint has a unique secret that is used to sign a payload delivered to you on each event. Steps to verify signature:

1. Extract signature from the request header
2. Compute HMAC with SHA256 function. Use your endpoint secret as key and JSON payload string as a message.
3. Compare signature in the header to the computed signature.

Here is a Node.js pseudo code to generate and compare signatures

```javascript
const secret = 'my-webhook-secret';
function generateSignature(string) {
  return require('crypto')
    .createHmac('sha256', secret)
    .update(string)
    .digest('base64');
}
```

And verifying signature

```javascript
function myCallbackEndpointHandler(req, res) {
  const signature = req.headers['flashfx-signature'];
  const payload = req.body;
  
  if (generateSignature(payload) !== signature) {
    console.error("Security warning! Webhook endpoint received bad data", req);
    res.sendStatus(500);
    return;
  }
  
  // proceed with the webhook processing
}
```

**Testing your endpoint**

To make sure your webhook endpoint is setup correctly, you can make a test HTTP request from the [FlashConnect](https://connect.flash-fx.com) webhook settings. Click the **"Send Test”** button. You should receive a dummy request, which will need to be acknowledged the same way as your real webhook requests.

### Subscribing to the events

Webhook requests come with event data. You can see all available event types in the [FlashConnect ](https://connect.flash-fx.com)settings. To subscribe, select which events you would like to receive and specify your endpoint. Please note, your endpoint must adhere to the HTTPS protocol for security reasons.

### Example payloads

**Deposits**

{% tabs %}
{% tab title="deposit_initiated" %}
<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "event": "deposit_cleared",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Deposit initiated",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
  "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}</code></pre>
{% endtab %}

{% tab title="deposit_cleared" %}
<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "event": "deposit_cleared",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Deposit cleared",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
  "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}</code></pre>
{% endtab %}

{% tab title="deposit_cancelled" %}
```json
{
  "event": "deposit_cancelled",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "currency": "AUD",
  "externalReference": "2233445566",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "statusMessage": "Cancelled by: john@example.com : ",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
    "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}

{% tab title="deposit_refunding" %}
```json
{
  "event": "deposit_refunding",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "refundAmount": 99,
  "currency": "AUD",
  "externalReference": "2233445566",
  "refundReason": "Client refund request",
  "statusMessage": "Deposit refunded",
  "refundedAt": "2021-03-03T03:28:43.936Z",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
    "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}

{% tab title="deposit_refunded" %}
```json
{
  "event": "deposit_refunded",
  "id": "603f0198770d6595e3c83e0d",
  "amount": 100,
  "totalFee": 1,
  "refundAmount": 99,
  "currency": "AUD",
  "externalReference": "2233445566",
  "refundReason": "Client refund request",
  "statusMessage": "Deposit refunded",
  "refundedAt": "2021-03-03T03:28:43.936Z",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "recipient": {
    "accountName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387"
  },
    "sender": {
    "accountName": "ACME Inc",
    "companyName": "ACME Inc",
    "bankName": "Bank ACME",
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}
{% endtabs %}

**Withdrawals**

{% tabs %}
{% tab title="withdrawal_initiated" %}
```json
{
  "event": "withdrawal_initiated",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}

{% tab title="withdrawal_completed" %}
```json
{
  "event": "withdrawal_completed",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "statusMessage": "Transaction Confirmed",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}

{% tab title="withdrawal_failed" %}
```json
{
  "event": "withdrawal_failed",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}

{% tab title="withdrawal_refunded" %}
```json
{
  "event": "withdrawal_refunded",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "refundAmount": 2000,
  "currency": "AUD",
  "externalId": "12344321",
  "refundReason": "Payout reversal of D.3eig for John Smith due to No account or incorrect account number",
  "statusMessage": "Payout reversal of D.3eig for John Smith due to No account or incorrect account number",
  "refundedAt": "2021-03-04T15:21:11.920Z",
  "clearedAt": "2021-03-03T03:25:12.792Z",
  "recipient": {
    "displayName": "John Smith",
    "bsb": "012620",
    "accountNo": "89900998"
  },
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "John Smith",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}

{% tab title="withdrawal_cancelled" %}
```json
{
  "event": "withdrawal_cancelled",
  "id": "51711af8c078ba061f623531",
  "amount": 2000,
  "totalFee": 1,
  "currency": "AUD",
  "externalId": "12344321",
  "statusMessage": "Withdrawal cancelled",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}
{% endtabs %}

**Payments**

{% tabs %}
{% tab title="currency_converted" %}
```javascript
{
  "event": "currency_converted",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="payment_complete" %}
```javascript
{
  "event": "payment_complete",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="payment_failed" %}
```javascript
{
  "event": "payment_failed",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="payment_cancelled" %}
```javascript
{
  "event": "payment_cancelled",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 1000,
  "fromCurrency": "AUD",
  "toAmount": 411.04,
  "toCurrency": "EUR",
  "externalId": "12344321"
}
```
{% endtab %}

{% tab title="payment_created" %}
```javascript
{
  "event": "payment_created",
  "id": "60711af8c078ba061f623531",
  "fromAmount": 3500,
  "fromCurrency": "EUR",
  "toAmount": 2501.94,
  "toCurrency": "AUD",
  "subClient": {
    "id": "203af01936410fd5d5e3c8f14d",
    "fullName": "ACME Inc",
    "accountNo": "1839394",
    "bsb": "809387",
    "externalId": "111222333"
  }
}
```
{% endtab %}
{% endtabs %}
