---
description: Withdrawal processing statuses
---

# Withdrawal statuses

1. You create a withdrawal. Your **balance goes down** by the withdrawal amount plus fee.\
   `INITIALISED`
   1. We might **manually** review the transaction.\
      `INITIALISED`-> <mark style="color:orange;">`REVIEWING`</mark>
   2. If review goes well it will become pending.\
      <mark style="color:orange;">`REVIEWING`</mark>→ `PENDING`
   3. Otherwise it gets cancelled.\
      <mark style="color:orange;">`REVIEWING`</mark>→ `CANCELLED`
2. The transaction is sent to the recipient bank for processing.\
   `INITIALISED` → `PENDING`
3. `PENDING` →
   * `CONFIRMED` - _the recipient bank have the money now_. Does not mean the beneficiary account number was credited though. Mostly _**FINAL status**_. Except when bank decides to return the funds, the withdrawal gets refunded (see step 4).
   * `FAILED` - NOT FINAL status. Depending on the processing error we have three scenarios now.
     * `REFUNDED` - see step 4 below.
     * `CANCELLED` - manual action. Your **balance goes up** by the amount, Flash Payments keeps the fee. _**FINAL status.**_
     * `PENDING` - rare case - retrying. Sometimes it might work. Go to item 3.
4. The recipient bank decided to return this transaction back to Flash Payments.\
   `CONFIRMED` → `FAILED` → `REFUNDED` - usual way - automatic refunding. Your **balance goes up** by the withdrawal amount, Flash Payments keeps the fee. _**FINAL status**_.

{% hint style="warning" %}
The <mark style="color:orange;">REVIEWING</mark> is an **optional** **manual** action by Flash Payments Compliance team. Occasionally we pick some transactions for extended AML/CT review. Most transactions do not ever get into the <mark style="color:orange;">REVIEWING</mark> status.
{% endhint %}

### Most common status transitions

#### Happy path

`INITIALISED`→<mark style="color:orange;">`REVIEWING`</mark><mark style="color:orange;">→</mark>`PENDING`→`CONFIRMED`

#### Unhappy paths

Recipient bank accepts the payout but then returns it:

`INITIALISED`→`PENDING`→`CONFIRMED`→`FAILED`→`REFUNDED`

Recipient bank rejects the payout:

`INITIALISED`→`PENDING`→`FAILED`→`REFUNDED`

Flash Payments Compliance team rejects the payout:

`INITIALISED`→`REVIEWING`→`CANCELLED`

### Testing/simulating unhappy paths

Typically, you want to adapt your system to handle the following common withdrawal scenarios:

#### 1. Too long in the <mark style="color:orange;">REVIEWING</mark> status

We are doing a manual review of your withdrawal, we sent your Compliance Team (or else) an email message requesting more information. In this case the withdrawal can be in <mark style="color:orange;">`REVIEWING`</mark> state for up to 10 days (5 business days + 2 weekends + an occasional holiday).&#x20;

{% hint style="info" %}
Please always provide accurate sender and recipient information, including the full address, to prevent delays associated with manual compliance reviews.
{% endhint %}

You can simulate the behaviour. Your `externalReference` must include this text: `HALT_AML`. For example: `"testing HALT_AML attempt 2"`. The withdrawal will get stuck in <mark style="color:orange;">`REVIEWING`</mark> forever.

#### 2. Fails AML and gets cancelled

We checked the data you sent us for withdrawal. We didn't like it. So we cancel it immediately.

You can simulate the behaviour. Your `externalReference` must include this text: `FAIL_AML`. For example: `"testing FAIL_AML attempt 4"`. The withdrawal will be cancelled next moment after you submit it.

#### 3. Recipient bank rejects the money

An often scenario in Australian domestic payments system is when a local transaction went seemingly fine. However, few days later a recipient bank decides to return the money. There are multiple reasons for that. For example: account number does not exist, account was closed, account is of a wrong currency, etc.

You can simulate the behaviour. Your `externalReference` must include this text: `FAIL_ACC`. For example: `"testing FAIL_ACC attempt 8"`. The withdrawal will be completed, and next moment failed and refunded.
