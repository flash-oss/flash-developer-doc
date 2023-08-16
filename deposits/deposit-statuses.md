---
description: Deposit processing statuses
---

# Deposit statuses

As soon as we see a deposit in a Flash Payments controlled Virtual Account Number (VAN) we create a deposit.

`INITIALISED`-><mark style="color:orange;">`REVIEWING`</mark><mark style="color:orange;">→</mark>`CONFIRMED`

If you choose to reject that deposit it goes through refunding statuses:

`INITIALISED`-><mark style="color:orange;">`REVIEWING`</mark><mark style="color:orange;">→</mark>`CONFIRMED`→`REFUNDING`→`REFUNDED`&#x20;

{% hint style="warning" %}
The <mark style="color:orange;">REVIEWING</mark> is an **optional** **manual** action by Flash Payments Compliance team. Occasionally we pick some transactions for extended AML/CT review. Most transactions do not ever get into the <mark style="color:orange;">REVIEWING</mark> status.
{% endhint %}

If Flash Payments Compliance choose to reject that deposit it goes through following statuses:

`INITIALISED`→`REVIEWING`→`REFUNDING`→`REFUNDED`&#x20;
