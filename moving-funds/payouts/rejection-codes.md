---
description: Examples of transaction cancellation static codes and standard status messages
---

# Rejection codes examples

If your transaction is canceled by our Compliance team, the unique `rejectCode` and a corresponding standard `statusMessage` will be assigned to this transaction and sent to your application via proper webhook such as [withdrawal\_canceled](https://developer.flash-payments.com/webhooks#withdrawal_cancelled) to be used for a subsequent integration action such as field mapping or reconciliation.&#x20;

Below is a short list of the most frequently used rejection codes and standard status messages. Please note that this list is incomplete and is provided for your information only, so you know what feedback to expect from the system. \
\
Please also note that while the rejection codes are unique, you might see the corresponding free text status messages customised by our Compliance team on a case-by-case basis for more accuracy and clarity of the information exchange.&#x20;

<table><thead><tr><th width="441">rejectCode</th><th width="648">statusMessage</th></tr></thead><tbody><tr><td><code>CANCELLATION_REQUESTED_BY_PARTICIPANT</code></td><td>The transaction is rejected upon request.</td></tr><tr><td><code>COMPLIANCE_DECLINED</code></td><td>The transaction is rejected as it doesn't meet our compliance requirements.</td></tr><tr><td><code>RECIPIENT_ACCOUNT_CLOSED</code></td><td>The transaction is declined due to blocked or closed account.</td></tr><tr><td><code>DATA_ENQUIRY_TIME_ELAPSED</code></td><td>The transaction is rejected due to no response received within the specified time.</td></tr><tr><td><code>DATA_ENQUIRY_RESPONSE_INADEQUATE</code></td><td>The transaction is rejected because the response was received with incomplete or poor-quality data.</td></tr></tbody></table>

{% hint style="danger" %}
**Please note**: status `CANCELLED` is a **final** status for **any** withdrawal. \
Please do not retry a withdrawal if it was rejected by our Compliance team or by a beneficiary financial institution.

Only those withdrawals rejected with codes`TECHNICAL_ISSUE` may be re-submitted as new transactions.
{% endhint %}
