---
description: Explains how to accept deposits programmatically
---

# Deposits

After fully registering with us, you get a BSB and a dedicated Virtual Account Number (VAN).

{% hint style="warning" %}
IMPORTANT: Your Australian VAN will be restricted to local Australian transfers. For international payments (aka FX payments), we offer different solutions.
{% endhint %}

Every deposit to your VAN would increase your Flash Payments balance.

Every deposit can trigger a [Webhooks](../webhooks/regular-webhooks.md) notification, keeping you informed in real time

The deposit data includes the payment reference (we call it `externalReference` in this API).

By default, only you are permitted to make deposits. However, we can enable third-party deposits for your account upon request. Once enabled, your account number functions as a local collection account, allowing others to deposit funds on your behalf.

We can also enable the [sub-client](../sub-clients.md) feature. It allows you to programmatically create client accounts with dedicated BSB and account number for transactional purposes. You can give these bank account details to your clients to accept deposits. Once funds arrive, we will increase your account balance, and you will see a deposit with sub-client information linked to it.

To browse your deposits, you can use our [FlashConnect](https://connect.uat.flash-payments.com.au/login) interface.

{% hint style="info" %}
Tip. You can simulate and test a deposit with the FlashConnect tool in the UAT environment. Just go to the _Deposits_ page and click "SEND TEST DEPOSIT".\
\
Additionally, you can test a deposit sent by your [sub-client](../sub-clients.md) in the UAT. Just go to the _Sub-clients_ page, find the sub-client, and click "SEND TEST DEPOSIT".
{% endhint %}
