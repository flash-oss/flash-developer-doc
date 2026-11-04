---
description: Sub-client VANs lifecycle
---

# Sub-client statuses

A sub-client lifecycle follows these stages.

A sub-client is created and has the **temporary** `INITIATED` status.\
At this stage, the request has been received and the system is currently processing it.

If processing is completed successfully, the sub-client automatically transitions to the `ACTIVE`.\
In this status, the sub-client is fully activated and approved.

If the sub-client requires manual compliance review, the sub-client moves from `INITIATED` to the **temporary** <mark style="color:orange;">`UNAPPROVED`</mark> status.\
From this status it would eventually transition to either `ACTIVE` or `FAILED_KYC`.\
Typically, we can't disclose the exact reason of the KYC failure, while it mostly happens because we do not have risk appetite for your client.

You can deactivate the client (via API of Flash Connect), - it'd move from `ACTIVE` to `DEACTIVATED`.\
A sub-client in the `DEACTIVATED` status can be activated by you at any time, returning to `ACTIVE`.

At any point, a sub-client may be moved to `DISABLED`.\
In this status, the sub-client is disabled and cannot be enabled by you, and contacting support is required for further information.
