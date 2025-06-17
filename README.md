---
description: Flash Payments Developer API documentation
---

# Overview

## General information

Flash Payments API is [GraphQL](http://graphql.github.io/learn/queries/)-based, offering simple and developer-friendly querying experience. All data is exchanged in JSON format.

Flash Payments API playground is located here: [https://api.uat.flash-payments.com.au/](https://api.uat.flash-payments.com.au/)

### Complete API docs

This documentation website **does not have full list of API** fields and methods. This is intentional. The full list of the API calls you can performs and the data fields you can send/receive is listed in the [API Playground](https://api.uat.flash-payments.com.au/) (click "**DOCS**" on the right hand side).

## High level feature overview

You can search, visualise, or extract your data using our [FlashConnect](https://connect.uat.flash-payments.com.au/) product.

### Instant local Australian deposit (aka pay-in)

Once registration is complete, a dedicated BSB and Virtual Account Number (VAN) for use within Australia will be assigned to you.

{% hint style="warning" %}
IMPORTANT: Your Australian VAN will be restricted to local Australian transfers. For international payments (aka FX payments), we offer different solutions. 
{% endhint %}

Any funds deposited to your VAN would increase your Flash Payments balance. We store and retain the reference attached to each deposit (e.g., an invoice number) to support your reconciliation or reporting needs.

By default, only you can deposit into your VAN. Third-party deposits are also supported, but this feature must be enabled separately

Deposits are typically processed in real time, but depending on the bank, there may be a delay of up to 24 hours.

You can configure webhook notifications to receive alerts for every deposit made to your VAN. Please visit [FlashConnect](https://connect.uat.flash-payments.com.au/) to set up your deposit webhook.

You can manually reject unwanted deposits via the [FlashConnect](https://connect.uat.flash-payments.com.au/) interface. The funds will be returned to the original sender bank account.

### Local Australian withdrawal (aka pay-out)

The Flash Payments API enables you to withdraw funds from your Flash Payments balance. By default, only you are authorized to receive these withdrawals. Third-party withdrawals (also known as payouts) are available upon request and require separate activation.

If your payout involves a foreign exchange (FX) payment, it must be processed using the Australian Direct Entry system in accordance with legal requirements. Processing time may vary from immediate to a few hours. Other payouts are credited instantly.

If a payout fails, you will receive a webhook notification detailing the reason for the failure.

The payout remitter name is configurable. You can give us the remitter name as `withdrawal.sender` data property. This is especially useful for FX-linked payouts. If a Brazilian mama Katarina Oreiro sends money to her son in Australia, he will see his mom's name in the bank statement - "Payment from Katarina Oreiro".

Payouts can also be done via the [FlashConnect](https://connect.uat.flash-payments.com.au/) interface.

### Send or receive money internationally

You can send your Flash Payments balance internationally via our API and benefit from instant delivery to many countries with local instant payment systems (e.g. Philippines)

To initiate a payment, you must pre-create the sender and recipient via the API.

All entities in our database, including withdrawals, payments, senders, and recipients, can store your system’s unique identifier. Please refer to the `externalId` field in the [API docs](https://api.uat.flash-payments.com.au/).

Payment delivery times depend on the recipient's country. While many are processed quickly, some may take additional time. You’ll be notified via webhook whenever the payment status changes.

You can send funds through an international payment to your Flash Payments account. A webhook notification will be triggered when we detect payments from other countries.

### Security

The API token you generate expires in 4 hours. You can always use `logout` GraphQL mutation to expire it earlier.

You can't reset your own password until we verify your identity.

The [FlashConnect](https://connect.uat.flash-payments.com.au/login) interface supports Google and One-Time-Password logins.

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

