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

