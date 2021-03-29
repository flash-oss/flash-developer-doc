---
description: Disburse your money to Australian bank account(s)
---

# Withdrawals

You can [retrieve](query-withdrawals.md) all or some of your withdrawals.

You can [create withdrawals](withdraw-funds.md). By default you can withdraw only to your personal bank account.

If you are a fully verified customer you can payout to any bank account. You'd need to:

* have a recipient ID \(see [Recipients](../recipients/) to query your address book\); otherwise
* understand the supported delivery methods using the [availableDeliveryMethods](../recipients/required-fields.md) query and [create a new recipient](../recipients/#create-a-recipient)

Having a recipient ID you can now withdraw money.

## Withdrawal processing states

1. You create a withdrawal. Your **balance goes down** by the withdrawal amount plus fee. \(none\) → `INITIALISED`
2. The transaction is sent to the recipient bank for processing. `INITIALISED` → `PENDING`
3. `PENDING` →
   1. `CONFIRMED` - the recipient has got the money on their balance. **FINAL state**.
   2. `FAILED` - NOT FINAL state. Depending on the processing error we have two scenarios now.
4. `FAILED` →
   1. `CANCELLED` - usual way - cancelling. Your **balance goes up** by the amount plus fee. **FINAL state**.
   2. `PENDING` - rare case - retrying. Sometimes it might work. Go to item 3.





