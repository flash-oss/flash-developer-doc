---
description: Flash Payments Developer API documentation
---

# Overview

## General information

Flash Payments API is [GraphQL](http://graphql.github.io/learn/queries/) based because GraphQL is simpler and easier than REST API and more developer friendly. Although, the data you send and receive is all JSON.

Flash Payments API playground is located here: [https://api.flash-payments.com/](https://api.flash-payments.com/)

### Complete API docs

This documentation website **does not have full list of API** fields and methods. This is intentional. The full list of the API calls you can performs and the data fields you can send/receive is listed in the [API Playground](https://api.flash-payments.com/) (click "**DOCS**" on the right hand side).

## High level feature overview

You can search, visualise, or extract your data using our FlashConnect product: [https://connect.flash-payments.com/](https://connect.flash-payments.com/)

### Instant local Australian deposit (aka pay-in)

You would get a dedicated Australian BSB and a bank account number.

{% hint style="warning" %}
Warning: The account number can only process local transfers, **no SWIFT/RTGS**.
{% endhint %}

Any funds deposited to that account would increase your Flash Payments balance. We preserve the provided payment reference of every deposit for your further utilisation. E.g. invoice number or else.

By default, only yourself is allowed to deposit to it. However, the third party deposits are also possible. Although, we would need to enable this setting for you separately.

Sometimes, banks can delay your deposit by up to 24 hours. This should be expected. But typically, deposits are reflected in your Flash Payments balance immediately.

If set, you would get a webhook (aka callback) notification about every deposit made to your bank account. Go to the [https://connect.flash-payments.com/](https://connect.flash-payments.com/) to setup a deposit webhook.

You can manually reject unwanted deposits via the [https://connect.flash-payments.com/](https://connect.flash-payments.com/) interface. The funds will be returned to the original sender bank account.

### Local Australian withdrawal (aka pay-out)

This API allows you to withdraw your Flash Payments balance. By default, only yourself is allowed to receive those funds. The third party withdrawals (aka payouts) are also possible. Although, we would need to enable this setting for you separately.

If your payout is a part of FX payments, we are legally obliged to use classic Australian payment system a.k.a. Direct Entry. It would take from 0 up to few hours to deliver such funds. Otherwise, payouts are delivered to the recipient instantly.

If a payout fails you will receive a webhook notification with a clear explanation of what went wrong. This is typically bank account number typos.

The payout remitter name is configurable. You can give us the remitter name as `withdrawal.sender` data property. This is especially useful for FX-linked payouts. If a Brazilian mama Katarina Oreiro sends money to her son in Australia, he will see his mom's name in the bank statement - "Payment from Katarina Oreiro".

Payouts can also be done via the [https://connect.flash-payments.com/](https://connect.flash-payments.com/) interface.

### Send or receive money internationally

You can send you Flash Payments balance internationally via our API and enjoy instant delivery to countries with local instant payment systems, e.g. Philippines. By the way, cash payments to Philippines are also supported.

Your code would need to pre-create both sender and recipient before creating a payment for them.

All the entities in our database (withdrawal, payment, sender, recipient) can hold your system's ID. See the `externalId` field in the [API docs](https://api.flash-payments.com/).

Depending on the recipient's country a payment can take from few minutes to few days. You would receive a webhook notification when a payment state changes.

In most cases you can send money to your Flash Payments balance. This can be automated. You will receive a webhook notification when we see you sending to Flash Payments from other countries.

### Security

The API token you generate expires in 4 hours. You can always use `logout` GraphQL mutation to expire it earlier.

You can't reset your own password until we verify your identity.

The [https://connect.flash-payments.com/](https://connect.flash-payments.com/) interface supports Google and One-Time-Password logins.

Webhooks have cryptographic signatures.

IP address whitelisting feature is available from FlashConnect interface allowing login to our APIs from only preapproved addresses that you specify.

API rate limiting is in place to ensure stable performance of our service and prevent its potential abuse.

## How to start

1. Contact with us via [this page](https://flash-payments.com/connect) or by clicking the Intercom button on the bottom right of this page.
2. Explain what kind of services you are looking from Flash Payments.
3. We will examine your needs and explain how to get access to our UAT environment.

For more detailed instructions head to the [Basics](basics/) page.

## Important notes

### Breaking changes

While we will endeavour to not introduce any breaking changes they might still occur in the future. In that case we will communicate about the upcoming changes via your registered email.

