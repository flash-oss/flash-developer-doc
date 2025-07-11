---
description: Generate Virtual Account Numbers for your clients for collection
---

# Sub-clients

The sub-client (aka _merchant_) feature allows you to create client accounts for deposit collection purposes. These virtual accounts can be issued to individuals, companies, or other organisations.

{% hint style="info" %}
Note: This feature is **disabled** by default. Contact us if you want it.
{% endhint %}

Each sub-client will receive a dedicated **BSB** and **Vitrual Account Number** (VAN) that you or your clients can use to accept and withdraw domestic AUD transfers within Australia.

{% hint style="warning" %}
IMPORTANT: Your Australian VAN will be restricted to local Australian transfers. For international payments (aka FX payments), we offer different solutions.
{% endhint %}

The **account name** is your sub-client's name. For companies - it's their `tradingAsName` or `legalName`. For individuals - it's their `fullName` (`firstName` + `middleName` + `lastName`).

If your Flash account has a multi-currency feature enabled, each of your sub-clients will also have access to virtual accounts in the selected currencies for which you hold balances.

All [deposits](https://developer.flash-payments.com/deposits) sent to your sub-client Virtual Account Numbers (VANs) are booked on your (master-client's) corresponding account balances. **Sub-clients can't have their own balances**.

You can[ disburse funds](https://developer.flash-payments.com/withdrawals/withdraw-funds) and [make FX payments](https://developer.flash-payments.com/payments/send-funds) on behalf of your sub-clients by providing the sub-client ID when you create withdrawals or payments. This sub-client record will then be used as the sender, linked to the transaction, and reported to the government.

Notifications via [webhooks](webhooks/webhooks.md) will provide important sub-client information as well.
