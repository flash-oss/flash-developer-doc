---
description: Answers to the frequently asked questions about payouts
---

# Payouts FAQ

This page answers the questions we get most often about the payouts you make with [createWithdrawal](withdraw-funds.md). Some words on this page have a special meaning:

* **Payout** and **withdrawal** have the same meaning: money that you send from your Flash balance to a bank account. The API uses the word withdrawal.
* **NPP** and **Direct Entry (DE)** are the two methods to send money to an Australian bank account. NPP is immediate and operates 24 hours a day, 7 days a week. DE, also known as BECS, sends payments in batches on business days. We select the method automatically. The `rail` field of the withdrawal shows the method that we used: `AU_NPP` or `AU_BECS`.
* A **webhook** is a message that we send to your server when the status of a payout changes. The name of the webhook, for example `withdrawal_cancelled`, shows the new status.

If an answer does not say differently, it applies to AUD payouts to Australian bank accounts. International payouts follow the rules of the payment system in the country of the recipient. For reference, see [Withdrawal statuses](withdrawal-statuses.md), [Rejection codes](rejection-codes.md) and [Webhooks](../../basics/webhooks/#withdrawal_initiated). We will add new questions and answers to this page when they occur. If your question is not here, ask us.

{% hint style="info" %}
**In short:** when you create a payout, we immediately reject it or accept it. If we accept it, we move it to its final status. We never hold a payout while we wait for money to come to your balance.
{% endhint %}

#### Balance <a href="#user-content-balance" id="user-content-balance"></a>

<details>

<summary> <strong>Q: What happens when my balance is too low?</strong></summary>

We reject the withdrawal immediately. `createWithdrawal` returns `success: false` and `code: "NO_BALANCE"`. We do not create a withdrawal record, we do not set a status, and we do not send a webhook. We do not hold anything, and nothing has a time limit. Add money to your balance and send the request again.

When we accept a withdrawal, we immediately take the amount and the fee from your balance (status `INITIALISED`). Thus, an accepted withdrawal is always funded.

</details>

#### Creating withdrawals and retrying <a href="#user-content-creating-withdrawals-and-retrying" id="user-content-creating-withdrawals-and-retrying"></a>

<details>

<summary><strong>Q: Can I retry <code>createWithdrawal</code> with the same <code>idempotencyKey</code>?</strong></summary>

Yes. If we rejected the first call, for example with `NO_BALANCE` or a validation error, you can send it again with the same `idempotencyKey`.

The `idempotencyKey` is a unique value that you send with each `createWithdrawal` call. If you send the same call again, the `idempotencyKey` prevents a second payout. If the first call was successful, we block the `idempotencyKey` for 24 hours. In these 24 hours, a second `createWithdrawal` with the same `idempotencyKey` returns the error `DUPLICATE_IDEMPOTENCY_KEY`.&#x20;

If you got no response, for example because of a timeout or a lost connection, you do not know if we created the withdrawal. If you send the call again, it can return `DUPLICATE_IDEMPOTENCY_KEY`. In this case the withdrawal possibly exists. Before you do anything else, find the withdrawal with the [`withdrawals`](query-withdrawals.md) query filtered by your `externalId`. This is possible only if you always send an `externalId`.

</details>

<details>

<summary><strong>Q: What happens if I submit the same <code>externalId</code> twice?</strong></summary>

The `externalId` is your reference for the payout, for example the transaction ID in your system. We reject the second call with `success: false` and `code: "EXTERNAL_ID_EXISTS"`. We do not create a record. You can use an `externalId` only one time in your account, and it has no time limit.

The `idempotencyKey` protects you for 24 hours. The `externalId` protects you permanently. Thus:

* Send your transaction ID as the `externalId`.
* To find the original withdrawal, use [`withdrawals`](query-withdrawals.md) query, filtered by `externalId`.
* When you send the same withdrawal again, do not submit a new `externalId` with it.

</details>

#### Delivery <a href="#user-content-delivery" id="user-content-delivery"></a>

<details>

<summary><strong>Q: What happens during a payment infrastructure or recipient bank outage?</strong></summary>

We accept the withdrawal, do our automated checks, and set the status to `PENDING`. The status stays `PENDING` while we deliver the payout. We do not reject a withdrawal because a payment system is not available, and `PENDING` has no time limit. The next step is different for each type of problem:

* **The recipient bank is not connected to the NPP network.** We send the payout by DE. The `rail` field of the withdrawal becomes `AU_BECS` instead of `AU_NPP`. The status stays `PENDING` until the DE transaction is complete. Note: DE cuts your `externalReference` to 18 characters. See [Withdraw funds](withdraw-funds.md#payment-reference-externalreference).
* **The recipient bank, or our banking partner, rejects the payout for a temporary reason** (for example, scheduled maintenance). We send the payout again automatically. Each time, we wait longer before we send it again. If all of these attempts fail, we set the status to `FAILED` and then `REFUNDED`, with `rejectCode: TECHNICAL_ISSUE`.

We send you one `withdrawal_pending` webhook when the status becomes `PENDING`. When we retry the payout, the status does not change, so we do not send more webhooks. The next webhook we send is `withdrawal_completed`, or `withdrawal_failed` and then `withdrawal_refunded`.

</details>

#### Compliance review

<details>

<summary><strong>Q: How long can a withdrawal stay in <code>REVIEWING</code>, and does it expire?</strong></summary>

Sometimes our automated checks send a withdrawal to our Compliance team for review. The status becomes `REVIEWING`, and we send you the `withdrawal_reviewing` webhook. A Compliance analyst then releases the withdrawal (status `PENDING`, then `CONFIRMED`) or declines it (status `CANCELLED`). Check the `rejectCode` and `statusMessage` to understand why this transaction was declined. If the analyst declines the payout, we return the amount to your balance and keep the fee.

Our Compliance team works in Sydney business hours. The time for a review is different for each case. The best way to prevent a manual review is to submit accurate sender and recipient details, with full addresses.

The `REVIEWING` status has no time limit. A timer starts only when our Compliance team sends you a [Request for Information (RFI)](../../compliance/request-for-information/), a set of questions about the withdrawal, which you must answer. The deadline of the RFI is 7 business days. If you do not answer in 8 business days, we cancel the withdrawal with `rejectCode: DATA_ENQUIRY_TIME_ELAPSED` and return the amount to your balance.

</details>

#### Failures, returns and refunds <a href="#user-content-failures-returns-and-refunds" id="user-content-failures-returns-and-refunds"></a>

<details>

<summary><strong>Q: Does Flash ever move a payout from <code>FAILED</code> back to <code>PENDING</code>?</strong></summary>

Yes, but rarely. This occurs only when our team examines a failed payout, addresses the cause, and sends the payout again. For example, a bank can reject a payout for a reason that we did not see before. If a withdrawal stays in `FAILED` for more than a few minutes, we already examine it. You do not have to do anything.

</details>

<details>

<summary><strong>Q: Can a <code>CONFIRMED</code> withdrawal still fail?</strong></summary>

Yes, but not often. The meaning of `CONFIRMED` is different for each delivery method:

* **NPP**: the money is at the recipient bank, and the bank accepted it. In some cases, the bank or the recipient asks us later to return the money.
* **DE**: we sent the payout to the recipient bank. The bank then processes the payout and can send it back, usually in one to three business days. The usual cause is an account that is not valid or is closed.

In both cases, the status goes from `CONFIRMED` to `FAILED` to `REFUNDED`, with a `rejectCode` such as `RECIPIENT_INVALID_ACCOUNT`, `RECIPIENT_ACCOUNT_CLOSED` or `CANCELLATION_BY_RECIPIENT_FI`. We return the amount to your balance.

</details>

<details>

<summary><strong>Q: How do I link a return or refund to the original payout?</strong></summary>

A return is not a new transaction. The status of the first withdrawal changes to `REFUNDED`. Thus, all the data that you need is in the withdrawal that you already have:

* The `withdrawal_failed` and `withdrawal_refunded` webhooks have the same `id` that `createWithdrawal` returned, as well as your `externalId`. The `withdrawal_failed` webhook also has the `rejectCode` and the `statusMessage`. The `withdrawal_refunded` webhook includes `refundAmount`, `refundReason`, `refundedAt`, and the BSB and account number of the recipient.
* The `Withdrawal` object returns `status`, `rejectCode`, `statusMessage`, `rejectedAt` and `refundedAt`. The `withdrawals` query can filter by `externalId`.

We return the refunded amount to your balance automatically and keep the fee. Returns do not occur often. The usual cause is incorrect recipient details, so the usual `rejectCode` is an invalid or closed account.

</details>

#### Rejection codes <a href="#user-content-rejection-codes" id="user-content-rejection-codes"></a>

<details>

<summary><strong>Q: Is it safe to resubmit a withdrawal rejected with <code>TECHNICAL_ISSUE</code>?</strong></summary>

Yes. `TECHNICAL_ISSUE` appears only on a withdrawal with a final status, `CANCELLED` or `REFUNDED`, and the amount is already back on your balance when you receive it. Make a new withdrawal with a new `idempotencyKey` and a new `externalId`. The old `externalId` stays attached to the cancelled withdrawal. If you use it again, we return `EXTERNAL_ID_EXISTS`.

We use `TECHNICAL_ISSUE` when all our automatic retries failed. We also use it when there was a technical problem on our side or at our banking partner.

</details>

<details>

<summary><strong>Q: What do <code>EXPIRED</code> and <code>DATA_ENQUIRY_TIME_ELAPSED</code> mean?</strong></summary>

* `EXPIRED`: a user made the withdrawal in [Flash Connect](https://connect.flash-payments.com/), our web portal. The transactions of this user need approval from another account member, and nobody approved the withdrawal in 7 days. We cancel it and return the amount to your balance. Withdrawals that you make through the API do not wait for approval. Thus, you will not see this code through the API.
* `DATA_ENQUIRY_TIME_ELAPSED`: our Compliance team sent you a Request for Information (RFI) about this withdrawal, and you did not answer in 8 business days. We cancel the withdrawal and return the amount to your balance. To prevent this, answer the RFI through the [RFI API](../../compliance/request-for-information/answer-rfi-questions.md) or in the RFI form emailed to you.

</details>

#### Webhooks

<details>

<summary><strong>Q: Which webhooks will I receive, and in what order?</strong></summary>

We send webhooks if you set a `callbackUri` on the withdrawal, or if you turned on [regular webhooks](../../basics/webhooks/regular-webhooks.md) for your account. The sequence depends on the path:\
\
**Automated checks pass**\
`withdrawal_initiated` → `withdrawal_pending` → `withdrawal_completed`\
\
**Manual review, released**\
`withdrawal_initiated` → `withdrawal_reviewing` → `withdrawal_pending` → `withdrawal_completed`\
\
**Manual review, declined**\
`withdrawal_initiated` → `withdrawal_reviewing` → `withdrawal_cancelled`&#x20;

**The recipient's bank rejects the payout**\
`withdrawal_initiated` → `withdrawal_pending` → `withdrawal_failed` + `withdrawal_refunded`&#x20;

**The recipient's bank returns the payout after confirmation**\
`withdrawal_initiated` → (`withdrawal_reviewing`) → `withdrawal_pending` → `withdrawal_completed` → `withdrawal_failed` + `withdrawal_refunded`&#x20;

</details>

#### Testing <a href="#user-content-which-webhooks-will-i-receive-and-in-what-order" id="user-content-which-webhooks-will-i-receive-and-in-what-order"></a>

<details>

<summary><strong>Q: How do I test my payout outcomes properly before going live?</strong></summary>

In our UAT environment, you can simulate each result with a keyword in the `externalReference` field: a long review, a Compliance decline, an RFI, or a payout that completes and then comes back. See [Withdrawal outcomes simulation](withdrawal-outcomes-simulation.md).

</details>
