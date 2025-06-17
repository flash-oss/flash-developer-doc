---
description: Deposit processing statuses
---

# Deposit statuses

Upon detecting a deposit in a Flash Payments Virtual Account Number (VAN), we immediately record it as a deposit in our system.

The status of a successful deposit goes through the following lifecycle

`INITIALISED`-><mark style="color:orange;">`REVIEWING`</mark><mark style="color:orange;">→</mark>`CONFIRMED`

If you choose to reject a deposit, its status goes through the following lifecycle instead:

`INITIALISED`-><mark style="color:orange;">`REVIEWING`</mark><mark style="color:orange;">→</mark>`CONFIRMED`→`REFUNDING`→`REFUNDED`&#x20;

{% hint style="warning" %}
The <mark style="color:orange;">REVIEWING</mark> is an **optional** **manual** action by Flash Payments Compliance team. Occasionally we pick some transactions for extended AML/CT review. Most transactions do not ever get into the <mark style="color:orange;">REVIEWING</mark> status.
{% endhint %}

If Flash Payments Compliance choose to reject a deposit, its status lifecycle goes through following stages:

`INITIALISED`→`REVIEWING`→`REFUNDING`→`REFUNDED`&#x20;
