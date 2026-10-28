---
description: Answers to the frequently asked questions about payouts
---

# Payouts FAQ

This page helps you find answers to the questions we are most often asked about payouts made using [`createWithdrawal`](withdraw-funds.md). Unless an answer says otherwise, it applies to AUD payouts delivered in Australia via NPP or Direct Entry (BECS). International payouts follow the receiving rail's own rules. The reference pages remain [Withdrawal statuses](withdrawal-statuses.md), [Rejection codes](rejection-codes.md) and [Webhooks](../../basics/webhooks/#withdrawal_initiated). We add to this page as new questions come up. If yours is not here, please ask us.

{% hint style="info" %}
**In one line:** a payout is either rejected synchronously when you create it, or accepted and processed by us to a final status. We never queue a payout waiting for funds.
{% endhint %}

### Balance <a href="#user-content-balance" id="user-content-balance"></a>

#### Q: What happens when my balance is too low? <a href="#user-content-what-happens-when-my-balance-is-too-low" id="user-content-what-happens-when-my-balance-is-too-low"></a>

The withdrawal is rejected immediately and nothing is created. `createWithdrawal` returns `success: false`, `code: "NO_BALANCE"` and `message: "Insufficient funds for a withdrawal"`. There is no withdrawal record, no status and no webhook, so there is nothing to hold, release or expire. Top up the balance and submit again.

The check is on your available balance in the withdrawal `currency`. If you fund AUD through a [conversion](../../fx/conversions/) while the FX market is closed, the conversion waits for the market to open (Monday 8:00 am Sydney), and withdrawals created before then are rejected with `NO_BALANCE`. AUD deposits to your account via NPP settle around the clock.

Once a withdrawal is accepted, the amount plus the fee is debited from your balance at once (status `INITIALISED`), so an accepted withdrawal is always funded.

### Creating withdrawals and retrying <a href="#user-content-creating-withdrawals-and-retrying" id="user-content-creating-withdrawals-and-retrying"></a>

#### Q: Can I retry `createWithdrawal` with the same `idempotencyKey`? <a href="#user-content-can-i-retry-createwithdrawal-with-the-same-idempotencykey" id="user-content-can-i-retry-createwithdrawal-with-the-same-idempotencykey"></a>

Yes, when the first attempt failed. The `idempotencyKey` is reserved for your account for 24 hours, and a second `createWithdrawal` with the same key within that window is refused with a GraphQL error whose code is `DUPLICATE_IDEMPOTENCY_KEY` (the error's extensions carry `statusCode: 409` and the HTTP response itself is a normal GraphQL reply with an `errors` array). It does not return the original withdrawal. The key is released as soon as the mutation answers `success: false` or throws, so a retry after a rejection such as `NO_BALANCE` or a validation error is safe with the same key.

If you never received a response (timeout, dropped connection), you do not know whether the withdrawal was created, and a retry may come back with `DUPLICATE_IDEMPOTENCY_KEY`. Treat that as "possibly created" and look the withdrawal up with the [`withdrawals`](query-withdrawals.md) query filtered by your `externalId` before deciding anything. For that to work, always send an `externalId`.

#### Q: What happens if I submit the same `externalId` twice? <a href="#user-content-what-happens-if-i-submit-the-same-externalid-twice" id="user-content-what-happens-if-i-submit-the-same-externalid-twice"></a>

The second call is rejected with `success: false`, `code: "EXTERNAL_ID_EXISTS"` and nothing is created. `externalId` is unique within your account across all your withdrawals, with no expiry, so it is the durable guard against duplicates. The `idempotencyKey` only protects the 24-hour retry window. Use your own payout identifier as the `externalId`, use the `withdrawals` query filtered by `externalId` to find the original, and never generate a new `externalId` for a retry of the same payout.

### Delivery <a href="#user-content-delivery" id="user-content-delivery"></a>

#### Q: What happens during an NPP, bank or partner outage? <a href="#user-content-what-happens-during-an-npp-bank-or-partner-outage" id="user-content-what-happens-during-an-npp-bank-or-partner-outage"></a>

The withdrawal is accepted, passes our automated checks, moves to `PENDING` and stays `PENDING` while we deliver it. We do not reject a withdrawal because a rail is down, and `PENDING` has no time limit. What we do depends on where the problem is:

* **The recipient's bank is not reachable via NPP.** We send the payout by Direct Entry instead, either straight away when we know the account is not NPP-enabled or after NPP tells us so. The `rail` field on the withdrawal changes from `NPP` to `BECS`. The status stays `PENDING` until the Direct Entry payment is processed. Note that Direct Entry truncates your `externalReference` to 18 characters, see [Withdraw funds](withdraw-funds.md#payment-reference-externalreference).
* **The recipient's bank rejects the payout temporarily** (maintenance, participant unavailable). We retry automatically with an increasing delay. Only if every retry fails does the withdrawal become `FAILED` and then `REFUNDED` with `rejectCode: TECHNICAL_ISSUE`.

You receive one `withdrawal_pending` webhook when the withdrawal enters `PENDING`.The retries above are not status changes and do not produce further webhooks. The next webhook is `withdrawal_completed`, or `withdrawal_failed` followed by `withdrawal_refunded`.

### Compliance review

#### Q: How long can a withdrawal stay in `REVIEWING`, and does it expire? <a href="#user-content-how-long-can-a-withdrawal-stay-in-reviewing-and-does-it-expire" id="user-content-how-long-can-a-withdrawal-stay-in-reviewing-and-does-it-expire"></a>

A withdrawal goes to `REVIEWING` when our automated screening asks the Compliance team to look at it. You receive the `withdrawal_reviewing` webhook. The analyst either releases it (`PENDING`, then `CONFIRMED`) or declines it (`CANCELLED` with `rejectCode: COMPLIANCE_DECLINED`; the amount is returned to your balance; the fee is kept).

Reviews are handled by our Compliance team during Sydney business hours, and how long one takes depends on the case. Accurate sender and recipient details, including full addresses, are the best way to avoid a manual review in the first place.

`REVIEWING` does not expire on its own. The only timer starts when the Compliance team sends you a [Request for Information (RFI)](../../compliance/request-for-information/): the RFI carries its `deadline` (7 business days), and if it is still unanswered after 8 business days the withdrawal is cancelled with `rejectCode: DATA_ENQUIRY_TIME_ELAPSED` and the amount is returned to your balance.

### Failures, returns and refunds <a href="#user-content-failures-returns-and-refunds" id="user-content-failures-returns-and-refunds"></a>

#### Q: Does Flash ever move a payout from `FAILED` back to `PENDING`? <a href="#user-content-does-flash-ever-move-a-payout-from-failed-back-to-pending" id="user-content-does-flash-ever-move-a-payout-from-failed-back-to-pending"></a>

Rarely, yes. A payout moves from `FAILED` back to `PENDING` only after our team reviews it and resubmits it once the cause has been fixed. For example, a bank might reject a payout for a reason we haven’t encountered before. If you see a withdrawal in `FAILED` for more than a few minutes, we’re already working on it, so you don’t need to do anything.

#### Q: Can a `CONFIRMED` withdrawal still fail? <a href="#user-content-can-a-confirmed-withdrawal-still-fail" id="user-content-can-a-confirmed-withdrawal-still-fail"></a>

Rarely, yes, and what `CONFIRMED` means depends on the rail. Via NPP it means the payment has settled and the receiving bank has accepted it. Occasionally that bank, or the recipient, later asks for the payment to be returned. Via Direct Entry it means the payment has been dispatched to the receiving bank in a BECS file. The bank then processes it and can return it, typically within one to three business days, an invalid or closed account being the usual reason.\
\
That shows as `CONFIRMED` → `FAILED` → `REFUNDED`, with a `rejectCode` such as `RECIPIENT_INVALID_ACCOUNT`, `RECIPIENT_ACCOUNT_CLOSED` or `CANCELLATION_BY_RECIPIENT_FI` and the amount back on your balance. Treat `REFUNDED` and `CANCELLED` as the only statuses after which no money moves.

#### Q: How do I link a return or refund to the original payout? <a href="#user-content-how-do-i-link-a-return-or-refund-to-the-original-payout" id="user-content-how-do-i-link-a-return-or-refund-to-the-original-payout"></a>

A return is not a separate transaction. The original withdrawal itself moves to `REFUNDED`, so everything you need is on the record you already have:

* The `withdrawal_failed` and `withdrawal_refunded` webhooks carry the same `id` that `createWithdrawal` returned and your `externalId`; `withdrawal_failed` adds the `rejectCode` and `statusMessage`,  `withdrawal_refunded` adds `refundAmount`, `refundReason`, `refundedAt` and the recipient's BSB and account number.
* The `Withdrawal` type returns `status`, `rejectCode`, `statusMessage`, `rejectedAt` and `refundedAt`, and the `withdrawals` query filters by `externalId`.

The refunded amount is credited back to your balance automatically. The fee is kept. Returns are the exception rather than the rule, and almost always come down to the quality of the recipient's details, so an invalid or closed account is the usual `rejectCode`.

### Rejection codes <a href="#user-content-rejection-codes" id="user-content-rejection-codes"></a>

#### Q: Is it safe to resubmit a withdrawal rejected with `TECHNICAL_ISSUE`? <a href="#user-content-is-it-safe-to-resubmit-a-withdrawal-rejected-with-technical_issue" id="user-content-is-it-safe-to-resubmit-a-withdrawal-rejected-with-technical_issue"></a>

Yes. `TECHNICAL_ISSUE` is only ever set on a final status, `CANCELLED` or `REFUNDED`, and the amount is already back on your balance when you receive it. The original withdrawal will not be paid. Create a new withdrawal with a new `idempotencyKey` and a new `externalId`. The old `externalId` stays attached to the cancelled withdrawal and would be refused with `EXTERNAL_ID_EXISTS`.

We use `TECHNICAL_ISSUE` when the retries described above were exhausted, or on a technical failure on our side or our partner's.

#### Q: What do `TECHNICAL_ISSUE`, `EXPIRED` and `DATA_ENQUIRY_TIME_ELAPSED` mean? <a href="#user-content-what-do-technical_issue-expired-and-data_enquiry_time_elapsed-mean" id="user-content-what-do-technical_issue-expired-and-data_enquiry_time_elapsed-mean"></a>

* `TECHNICAL_ISSUE`: see the previous question. Safe to resubmit as a new withdrawal.
* `EXPIRED`: the withdrawal was created in [Flash Connect](https://connect.flash-payments.com/) by a member whose transactions need approval, and nobody approved it within 7 days. It is cancelled by a nightly job and the amount is returned to your balance. Withdrawals created through the API are straight-through and never wait for approval, so an API integration will not see this code.
* `DATA_ENQUIRY_TIME_ELAPSED`: the Compliance team sent you a request for information about this withdrawal and did not receive your answers within 8 business days. The withdrawal is cancelled and the amount returned to your balance. Answer RFIs through the [RFI API](../../compliance/request-for-information/answer-rfi-questions.md) or in Flash Connect to avoid this.

### Webhooks

#### Q: Which webhooks will I receive, and in what order? <a href="#user-content-which-webhooks-will-i-receive-and-in-what-order" id="user-content-which-webhooks-will-i-receive-and-in-what-order"></a>

For a withdrawal created with a `callbackUri` or with [regular webhooks](../../basics/webhooks/regular-webhooks.md) enabled:

<table><thead><tr><th width="249.328125">Path</th><th>Webhooks, in order</th></tr></thead><tbody><tr><td>Automated checks pass</td><td><code>withdrawal_initiated</code> → <code>withdrawal_pending</code> → <code>withdrawal_completed</code></td></tr><tr><td>Manual review, released</td><td><code>withdrawal_initiated</code> → <code>withdrawal_reviewing</code> → <code>withdrawal_pending</code> → <code>withdrawal_completed</code></td></tr><tr><td>Manual review, declined</td><td><code>withdrawal_initiated</code> → <code>withdrawal_reviewing</code> → <code>withdrawal_cancelled</code></td></tr><tr><td>Bank rejects the payout</td><td><code>withdrawal_initiated</code> → <code>withdrawal_pending</code> → <code>withdrawal_failed</code> + <code>withdrawal_refunded</code></td></tr><tr><td>Bank returns the payout after confirmation</td><td><code>withdrawal_initiated</code> →[<code>withdrawal_reviewing</code>]→ <code>withdrawal_pending</code> → <code>withdrawal_completed</code> → <code>withdrawal_failed</code> + <code>withdrawal_refunded</code></td></tr></tbody></table>

`withdrawal_failed` and `withdrawal_refunded` are sent together. The other events arrive in the order shown, usually within seconds of each other. The event name is in the payload of a `callbackUri` callback (`event` field) and in the regular webhook delivery, and each regular webhook delivery carries a `flashfx-request-id` header that stays the same across our retries, so you can ignore duplicates. Delivery retries and signature verification are described on the [Webhooks](../../basics/webhooks/) pages.

### Testing

#### Q: How do I test these outcomes before going live? <a href="#user-content-how-do-i-test-these-outcomes-before-going-live" id="user-content-how-do-i-test-these-outcomes-before-going-live"></a>

In UAT, put a keyword in `externalReference` to simulate a long review, an AML decline, an RFI, or a payout that completes and is then returned. See [Withdrawal outcomes simulation](withdrawal-outcomes-simulation.md).
