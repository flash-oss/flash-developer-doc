---
description: Disburse your money to a third party bank account
---

# Payouts

You can [create payouts](withdraw-funds.md) via the `createWithdrawal` mutation.

You can [retrieve](query-withdrawals.md) your payouts via the `withdrawals` query.

Sometimes you'd need to understand the supported delivery methods using the [availableDeliveryMethods](../recipients/delivery-methods.md) query.

If you get instructed by another financial institution then you must submit their details too.

If the payouts didn't pass compliance we would reject it with a proper rejection code and descriptive message.

Wondering what happens to a withdrawal during a bank outage, how long a compliance review can take, or when a withdrawal is final? See the [Payouts FAQ](payouts-faq.md).

