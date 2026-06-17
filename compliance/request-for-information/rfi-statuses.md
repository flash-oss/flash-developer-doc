---
description: RFI lifecycle statuses
---

# RFI statuses

Most common RFI status transitions follow one of these paths

**Happy path**

`PENDING` → `ASSESSING` → `CLOSED` <br>

1. It starts when our compliance review flags one of your transactions. An RFI is created with a list of questions and a response deadline.\
   \
   `PENDING` means the RFI is open for your response. \
   Answer each question via [`answerRfiQuestion`](answer-rfi-questions.md) before `deadline`. The status stays `PENDING` until all answers are received or the deadline passes.
2. The last open question is answered.\
   \
   `ASSESSING` means that all your answers have been received and are under our review. No action is required from you.
3. The review completes.\
   \
   &#x20;`CLOSED` means that the RFI is finalised. No further answers are accepted. _**Final status.**_

\
**Declined or expired path**

`PENDING` → `CLOSED`

An RFI can also move to `CLOSED` directly from `PENDING` when you [decline it](decline-an-rfi.md), or when the deadline passes without a complete response from your end.

\
**Canceled path**

`PENDING` → `CANCELLED`  or  `ASSESSING` → `CANCELLED`&#x20;

Separately from your responses, our compliance team can withdraw an RFI when the information is no longer needed — for example the underlying transaction question was resolved another way. The RFI moves to `CANCELLED`, no further answers are accepted, and any linked deposits, withdrawals, or payments are left untouched and continue on their own lifecycle. You don't need to do anything — we notify you by email and via the `rfi_cancelled` webhook. _**Final status.**_

{% hint style="info" %}
`CANCELLED` status is different from `CLOSED`. The status `CLOSED` is the outcome of a review (the linked transactions are then released or rejected). `CANCELLED` means we withdrew the request itself — the linked transactions are not affected by the cancellation.
{% endhint %}

Each transition emits a [webhook event](../../basics/webhooks/#rfi_created):

| Event           | Status      |
| --------------- | ----------- |
| `rfi_created`   | `PENDING`   |
| `rfi_assessing` | `ASSESSING` |
| `rfi_closed`    | `CLOSED`    |
| `rfi_cancelled` | `CANCELLED` |

**Status message**

The `statusMessage` field carries a meaningful explanation of the current status, for example "We are waiting for your replies" while `PENDING`, or "We assessed the information you provided" once `CLOSED`. The exact wording varies with the circumstances — treat it as display text, not as a value to parse.

{% hint style="info" %}
Once an RFI is `CLOSED`, the outcome is applied to the linked transactions: they are released, or rejected with an explanatory `rejectCode` (see [Rejection codes](../../moving-funds/payouts/rejection-codes.md)). The RFI itself does not expose the review verdict.
{% endhint %}
