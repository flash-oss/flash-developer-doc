---
description: Sub-client VANs lifecycle
---

# Sub-client statuses

A sub-client lifecycle follows these stages.

A sub-client is created and enters the `INITIATED` status.\
At this stage, the request to create a sub-client has been received and the system is currently processing it.

If processing is completed successfully, the sub-client transitions to `ACTIVE`.\
In this status, the sub-client is fully activated and approved.

If the sub-client requires manual compliance review, the sub-client moves from `INITIATED` to <mark style="color:orange;">`UNAPPROVED`</mark>.\
From this status it would eventually transition to either `ACTIVE` or `FAILED_KYC`. Typically, we can't disclose the exact reason of the KYC failure mostly because we do not have risk appetite for your client.

If the sub-client is deactivated by the user, it moves from `ACTIVE` to `DEACTIVATED`.\
A sub-client in `DEACTIVATED` status can be activated again at any time, returning it to `ACTIVE`.

At any point, a sub-client may be moved to `DISABLED`.\
In this status, the sub-client was disabled and cannot be enabled by the you, and contacting support is required for further information.
