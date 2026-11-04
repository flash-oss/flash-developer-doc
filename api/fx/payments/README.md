---
description: Convert and send your balance as single instruction
---

# FX Payments

What we call "FX Payment" is a fully orchestrated account to account delivery of any currency(-ies) we support. Typical examples:

* You need to send from the EU to Australia. You would have a European virtual IBAN on your name. As soon as you deposit some EUR to it - you'll see a corresponding amount of AUD on your master balance in a matter of minutes, which you'll be able to use immediately.
* You need to fund someone's overseas IBAN in Euros. You send us the AUD and give us the recipient's details either via API or via you FlashConnect back-office GUI. We orchestrate the rest.

You can [create payments](send-funds.md). using the `createPayment` mutation.

You can [retrieve](query-payments.md) your payments with the `payments` query.

But before you need to understand the supported delivery methods using the [availableDeliveryMethods](../../moving-funds/recipients/delivery-methods.md) query.
