---
description: How to secure your callback endpoints
---

# Ad hoc webhooks

When [sending a payment](../payments/send-funds.md) or [creating a local withdrawal](../withdrawals/withdraw-funds.md) you can provide us a webhook (callback) URI - `callbackUri`. We will call it when a payment or withdrawal status changes.

We recommend API clients to generate and add `?signature=ASecretPerPaymentKey` query to your `callbackUri` to make sure it's Flash Payments calling your webhook endpoint. For example:

```
https://my-webhooks.example.com/flash-payments?signature=oZaDlmfXbdXSKCnuWrvos2ImVBFX2Ru5
```

To avoid storing the signatures in a database we recommend generating them on the fly using a strong hash function or any kind of cryptography.

## Example

{% hint style="info" %}
This is just an example. Feel free to sign your URLs the way you want.
{% endhint %}

You would need to implement two functions.

1. Function to generate "signature".
2. Function to verify the "signature".

### Generating signatures

Node.js pseudo code for creating transfers in Flash Payments API.

```javascript
const secret = 'abcdefg';
function generateSignature(string) {
  return require('crypto')
    .createHmac('sha256', secret)
    .update(string)
    .digest('base64');
}

const signature = generateSignature(stringIdFromMyDatabase);
const callbackUri = 
  "https://my-webhooks.example.com/flashfx?signature=" + signature;
const externalId = stringIdFromMyDatabase;

// Use both callbackUri and externalId when creating transfers with Flash Payments API
```

The code above creates a `callbackUri` and `externalId` variables. Use both of them when creating a transfer in Flash Payments API.

### Verifying signatures

Node.js pseudo code of the webhook endpoint HTTP request handler.

```javascript
function myCallbackEndpointHandler(req, res) {
  const signature = req.query.signature;
  const stringIdFromMyDatabase = req.body.externalId;
  
  if (generateSignature(stringIdFromMyDatabase) !== signature) {
    console.error("Security warning! Webhook endpoint received bad data", req);
    res.sendStatus(500);
    return;
  }
  
  // proceed with the webhook processing
}
```
