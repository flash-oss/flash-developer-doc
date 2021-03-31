---
description: Two types of the webhooks
---

# Webhooks

There are two types of webhooks in FlashFX.

* [Regular webhooks](webhooks.md) - a URL would need to be saved to your [FlashConnect](https://connect.flash-fx.com/) settings. All types of events.
  * You can lookup the history of all the HTTP requests and responses, their JSON bodies and headers.
  * If there is no response we will show you what exactly the problem is: DNS issue, networking issue, 5XX response, etc.
* [Ad hoc webhooks](adhoc-webhooks.md) - you would need to provide a callback URL per payment/withdrawal while creating them. Only "payment" and "withdrawal" events are supported.



