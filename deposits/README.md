---
description: Explains how to accept deposits programmatically
---

# Deposits

After fully registering with us, you get a BSB and a dedicated Virtual Account Number (VAN).

{% hint style="warning" %}
Warning: The account number can only process local transfers, **no SWIFT/RTGS**.
{% endhint %}

Every deposit to this account would increase your Flash Payments balance.

You can receive notifications via [Webhooks](../webhooks/webhooks.md) about every deposit.

The deposit data includes the payment reference (we call it `externalReference` in this API).

By default, only yourself is allowed to deposit. However, we can enable third party deposits feature for your account. This, effectively, makes your account number a local collection account.

We can also enable the [sub-client](../sub-clients.md) feature. It allows you to programmatically create client accounts with dedicated BSB and account number for transactional purposes. You can give these bank account details to your clients to accept deposits. Once funds arrive, we will increase your account balance, and you will see a deposit with sub-client information linked to it.

To browse your deposits, you can use our FlashConnect tool: [https://connect.flash-payments.com/](https://connect.flash-payments.com/)

{% hint style="info" %}
Tip. You can simulate a deposit using the FlashConnect tool. Just go to the _Deposits_ page and click "SEND TEST DEPOSIT". It's available only in our development environment.\
Additionally, you can fake a deposit sent by your [sub-client](../sub-clients.md). Just go to the _Sub-clients_ page, find the sub-client, and click "SEND TEST DEPOSIT". It's available only in our development environment.
{% endhint %}
