---
description: Transaction cancellation static codes and standard status messages
---

# Rejection codes

If your transaction is canceled by our Compliance team, the unique `rejectCode` and a corresponding standard `statusMessage` will be assigned to this transaction and sent to your application via proper webhook such as [withdrawal\_canceled](https://developer.flash-payments.com/webhooks#withdrawal\_cancelled) to be used for a subsequent integration action such as field mapping or reconciliation.&#x20;

Below is the latest list of our supported rejection codes and standard status messages. Please note, while the **rejection codes are unique**, you might see the corresponding free text status messages customized by our Compliance team for more accuracy and clarity of the information exchange.&#x20;

<table><thead><tr><th width="441">rejectCode</th><th width="648">statusMessage</th></tr></thead><tbody><tr><td><code>CANCELLATION_REQUESTED_BY_PARTICIPANT</code></td><td>The transaction is rejected upon request.</td></tr><tr><td><code>CANCELLATION_BY_RECIPIENT_FI</code></td><td>The transaction is rejected by recipient's financial institution.</td></tr><tr><td><code>COMPLIANCE_DECLINED</code></td><td>The transaction is rejected as it doesn't meet our compliance requirements.</td></tr><tr><td><code>FRAUD</code></td><td>The transaction is rejected due to a fraud.</td></tr><tr><td><code>SENDER_INADEQUATE_DATA</code></td><td>The transaction is rejected as the sender name details are missing or invalid.</td></tr><tr><td><code>RECIPIENT_INADEQUATE_DATA</code></td><td>The transaction is rejected as the beneficiary name details are missing or invalid.</td></tr><tr><td><code>SENDER_INVALID_ADDRESS</code></td><td>The transaction is rejected as the sender address details are invalid.</td></tr><tr><td><code>SENDER_INCOMPLETE_ADDRESS</code></td><td>The transaction is rejected as the sender address details are missing.</td></tr><tr><td><code>RECIPIENT_INVALID_ADDRESS</code></td><td>The transaction is rejected as the beneficiary address details are invalid.</td></tr><tr><td><code>RECIPIENT_INCOMPLETE_ADDRESS</code></td><td>The transaction is rejected as the beneficiary address details are missing.</td></tr><tr><td><code>RECIPIENT_ACCOUNT_CLOSED</code></td><td>The transaction is declined due to blocked or closed account.</td></tr><tr><td><code>RECIPIENT_INVALID_ACCOUNT_NUMBER</code></td><td>The transaction is declined due to missing or invalid account number.</td></tr><tr><td><code>RECIPIENT_INVALID_BANK_CODE</code></td><td>The transaction is declined due to invalid BSB number.</td></tr><tr><td><code>RECIPIENT_INVALID_ACCOUNT</code></td><td>The transaction is declined as the currency cannot be applied to the specified account.</td></tr><tr><td><code>DUPLICATE_TRANSACTION</code></td><td>The transaction is declined as duplicate.</td></tr><tr><td><code>DATA_ENQUIRY_TIME_ELAPSED</code></td><td>The transaction is rejected due to no response received within the specified time.</td></tr><tr><td><code>TECHNICAL_ISSUE</code></td><td>The transaction is declined due to technical issue.</td></tr></tbody></table>

{% hint style="danger" %}
**Please note**: status `CANCELLED` is a **final** status for **any** withdrawal. \
Please do not retry a withdrawal if it was rejected by our Compliance team or by a beneficiary financial institution.

Only those withdrawals rejected with codes`TECHNICAL_ISSUE` or `CANCELLATION_REQUESTED_BY_PARTICIPANT`may be re-submitted as **new** transactions.
{% endhint %}
