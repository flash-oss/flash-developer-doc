---
description: Disburse your money to Australian bank account(s)
---

# Withdrawals

You can [retrieve](query-withdrawals.md) all or some of your withdrawals.

You can [create withdrawals](withdraw-funds.md). By default you can withdraw only to your personal bank account.

If you are a fully verified customer you can payout to any bank account. You'd need to:

* have a recipient ID (see [Recipients](../recipients/) to query your address book); otherwise
* understand the supported delivery methods using the [availableDeliveryMethods](../recipients/required-fields.md) query and [create a new recipient](../recipients/#create-a-recipient)

Having a recipient ID you can now withdraw money.

## Withdrawal processing statuses

1. You create a withdrawal. Your **balance goes down** by the withdrawal amount plus fee.\
   (none) → `INITIALISED`
2. The transaction is sent to the recipient bank for processing.\
   `INITIALISED` → `PENDING`
3. `PENDING` →
   * `CONFIRMED` - _the recipient bank have the money now_. Does not mean the beneficiary account number was credited though. Mostly _**FINAL status**_. Except when bank decides to return the funds, the withdrawal gets refunded (see step 4).
   * `FAILED` - NOT FINAL status. Depending on the processing error we have three scenarios now.
     * `REFUNDED` - see step 4 below.
     * `CANCELLED` - manual action. Your **balance goes up** by the amount, FlashFX keeps the fee. _**FINAL status.**_
     * `PENDING` - rare case - retrying. Sometimes it might work. Go to item 3.
4. The recipient bank decided to return this transaction back to FlashFX.\
   `CONFIRMED` → `FAILED` → `REFUNDED` - usual way - automatic refunding. Your **balance goes up** by the withdrawal amount, FlashFX keeps the fee. _**FINAL status**_.

### Most common status transitions

#### Happy path

`INITIALISED`→`PENDING`→`CONFIRMED`

#### Unhappy paths

Recipient bank accepts the payout but then returns it:

`INITIALISED`→`PENDING`→`CONFIRMED`→`FAILED`→`REFUNDED`

Recipient bank rejects the payout:

`INITIALISED`→`PENDING`→`FAILED`→`REFUNDED`

Our banking provider rejects the payout:

`INITIALISED`→`FAILED`
