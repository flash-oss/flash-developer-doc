---
description: Withdrawal rejection static codes and standard status messages
---

# Withdrawal rejection codes

After withdrawal is canceled by the Compliance team, the unique `rejectCode` and a corresponding standard `statusMessage` are assigned to this transaction and sent to your application via [`withdrawal_canceled`](https://developer.flash-payments.com/webhooks#withdrawal\_cancelled) webhook to be used for a subsequent action with your integration such as field mapping or reconciliation.&#x20;

Below is the latest list of our supported rejection codes and standard status messages. Please note, while the **rejection codes are unique**, you might see the corresponding status messages modified by the Compliance team for more clarity and accuracy.&#x20;

\


<table><thead><tr><th width="177">rejectCode</th><th width="648">statusMessage</th><th></th></tr></thead><tbody><tr><td>CANCELLATION_REQUESTED_BY_PARTICIPANT</td><td>The transaction is rejected upon request.</td><td></td></tr><tr><td>COMPLIANCE_DECLINED</td><td>The transaction is rejected as it doesn't meet our compliance requirements.</td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>
