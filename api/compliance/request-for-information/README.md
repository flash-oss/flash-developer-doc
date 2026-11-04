---
description: Respond to compliance information requests programmatically
---

# Request for Information

A Request for Information (RFI) is a compliance request raised by Flash Payments against deposits, withdrawals, or payments that require additional information before they can be released. RFIs are created on our side when a compliance review flags certain transactions — you cannot create RFIs via the API.

Each RFI carries a list of questions and a response deadline. There are two ways to respond:

* **Via the API** (this section) — receive a webhook, [query the RFI](query-rfis.md), [answer each questio](answer-rfi-questions.md)n with text or documents, or [decline](decline-an-rfi.md) if you cannot comply.
* **Via the secure form** — the end user fills in the secure form sent by email ([the existing flow](../../moving-funds/payouts/withdrawal-outcomes-simulation.md#id-4.-request-for-information-rfi-is-initiated-by-the-flash-compliance-team)). No integration required.

Both paths emit the same [lifecycle webhooks](../../basics/webhooks/#rfi_created), so you can mix them — for example, automate document collection but let the email form remain as a fallback.

{% hint style="info" %}
The RFI API is available to every client. There is no separate enablement step. Authentication uses your existing [API token](../../basics/authentication.md) — no new credentials.
{% endhint %}

### What's in this section

* [RFI statuses ](rfi-statuses.md)— the `PENDING` → `ASSESSING` → `CLOSED`  lifecycle and a `CANCELLED` case.
* [Query RFIs](query-rfis.md) — the `rfi` and `rfis` queries.
* [Answer RFI questions](answer-rfi-questions.md) — the `answerRfiQuestion` mutation, including file uploads.
* [Decline an RFI](decline-an-rfi.md) — the `declineRfi` mutation.
* [RFI response codes](rfi-response-codes.md) — all reply codes and GraphQL errors.

### Recommended integration flow

1. Subscribe to the `rfi_created`, `rfi_assessing`, `rfi_closed` and `rfi_canceled` [webhook events](../../basics/webhooks/#rfi_created) in your Flash Connect webhook settings.
2. On `rfi_created`, call the [`rfi`](query-rfis.md#retrieving-a-single-rfi) query to load the questions and the linked deposits, withdrawals, or payments.
3. For each question with `answered: false`, call [`answerRfiQuestion`](answer-rfi-questions.md) with text or files, according to the question's `fileUploadOption`.
4. When the last question is answered, the RFI moves to `ASSESSING` — you will receive `rfi_assessing`. No further action is required.
5. When the review is completed, the RFI moves to `CLOSED` and you will receive `rfi_closed`. The linked transactions are then released, or rejected with an explanatory `rejectCode`.
6. If you cannot supply the requested information, call [`declineRfi`](decline-an-rfi.md) while the RFI is still `PENDING`. This will cancel the linked transactions being still under review.
7. Separately from your responses, we may withdraw an RFI if it is no longer needed. If that happens the RFI moves to `CANCELLED` and you receive the `rfi_cancelled` webhook — no response is required, and any linked deposits, withdrawals, or payments are left unaffected.

{% hint style="info" %}
`CANCELLED` is initiated by us, not by you — it means we withdrew the request. It is different from declining (which you initiate, and which closes the RFI as `CLOSED` and cancels the linked transactions).&#x20;
{% endhint %}

{% hint style="warning" %}
Respond before the RFI's `deadline`. If the deadline passes without a complete response, the RFI is closed and the linked transactions may be rejected.
{% endhint %}
