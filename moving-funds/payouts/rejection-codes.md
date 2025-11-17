---
description: How to handle a payout rejection
---

# Rejection codes

If your withdrawal is canceled (aka rejected) by our AML and Compliance team, you'll receive the  [withdrawal\_cancelled](https://developer.flash-payments.com/webhooks#withdrawal_cancelled) webhook containing both a rejection code (`rejectCode`) and a manually written rejection reason (`statusMessage`). You can find the same two properties within the [`withdrawal`](query-withdrawals.md) GraphQL type.

Here is a curent list of all possible `rejectCode`s, but please note this list **can be extended with time**.

```
CANCELLATION_REQUESTED_BY_PARTICIPANT
CANCELLATION_BY_RECIPIENT_FI
COMPLIANCE_DECLINED
COMPLIANCE_DECLINED_EXTERNAL
SENDER_INADEQUATE_DATA
SENDER_INVALID_ADDRESS
SENDER_INCOMPLETE_ADDRESS
RECIPIENT_INADEQUATE_DATA
RECIPIENT_INVALID_ADDRESS
RECIPIENT_INCOMPLETE_ADDRESS
DATA_ENQUIRY_TIME_ELAPSED
DATA_ENQUIRY_RESPONSE_INADEQUATE
DUPLICATE_TRANSACTION
TECHNICAL_ISSUE
RECIPIENT_ACCOUNT_CLOSED
RECIPIENT_INVALID_ACCOUNT_NUMBER
RECIPIENT_INVALID_BANK_CODE
RECIPIENT_INVALID_ACCOUNT
RECIPIENT_COUNTRY_NOT_SUPPORTED
RECIPIENT_CURRENCY_NOT_SUPPORTED
EXPIRED
```

{% hint style="danger" %}
**Please note**: `CANCELLED` is a **final** status for **any** withdrawal. \
Please do not retry a withdrawal if it was rejected by our Compliance team or by a beneficiary financial institution.

Only those withdrawals rejected with code `TECHNICAL_ISSUE` may be re-submitted as new transactions.
{% endhint %}

Please also note that while the rejection codes are set, the corresponding free text `statusMessage` will have a case-by-case human written text explaining the root cause of the cancellation. We'll try to be as descriprive as possible taking into account the legal bounds (we are not allowed by the law to disclose certain information).
