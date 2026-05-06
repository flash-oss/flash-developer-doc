---
description: >-
  Test common payout outcomes in UAT, as you build and exercise your integration
  solution against the full range of real-life withdrawal flows, statuses and
  webhooks before going live.
---

# Withdrawal outcomes simulation

To trigger a happy or unhappy path scenario, put the corresponding keyword in the `externalReference` field when you submit a withdrawal. The keyword instructs our system to automatically simulate that outcome instead of routing the withdrawal through the standard compliance and payout pipelines, which may require manual intervention by a compliance analyst.\
\
Typically, you want to adapt your system to handle the following common withdrawal scenarios:&#x20;

#### 1. Too long in the <mark style="color:orange;">REVIEWING</mark> status

If your withdrawal is in this status this means that we are doing an internal review of it. It happens occasionally when you trigger our monitoring rules. Also, this means that we have sent your Compliance Team (or else) an email message requesting more information. You must respond. In other words, if there is a delay - it's on you. Because Flash Payments delivers all your transactions in real-time 24/7.

{% hint style="info" %}
Please always provide accurate [sender](https://developer.flash-payments.com/senders#create-a-sender) and [recipient](https://developer.flash-payments.com/recipients#create-a-recipient) information, including the full address, to prevent delays associated with internal compliance reviews on our side.

Sender and recipient addresses are automatically validated when a transaction is created. If the address cannot be verified, the transaction will be flagged for review by our Compliance team, resulting in processing delays.\
To help ensure smooth and timely processing, please provide complete and accurate address details.
{% endhint %}

You can simulate the behaviour. Your `externalReference` must include this text: `HALT_AML`. For example: `"testing HALT_AML attempt 2"`. The withdrawal will get stuck in <mark style="color:orange;">`REVIEWING`</mark> forever.

#### 2. Fails AML and gets cancelled

We checked the data you sent us for withdrawal. We didn't like it. So we cancel it immediately.

You can simulate the behaviour. Your `externalReference` must include this text: `FAIL_AML`. For example: `"testing FAIL_AML attempt 4"`. The withdrawal will be cancelled next moment after you submit it.

#### 3. Passes AML and gets processed&#x20;

If our smart AML/TM checks stop your withdrawal for manual review, you can have it automatically pushed forward by including `PUSH_AML` in the `externalReference`. For example: `"testing PUSH_AML attempt 5"`.\
\
This enables the transaction to continue as if it were approved by the compliance analyst. It can be particularly useful during your integration testing activities and also help with UAT regression testing later.

Please note that the <mark style="color:orange;">`REVIEWING`</mark> status for this payout will be recorded on our end, and a `withdrawal_reviewing` webhook will be sent.

#### 4. Request for Information (RFI) is initiated by the Flash Compliance Team

Whenever our Compliance team needs more details about specific withdrawal, you’ll receive an email with a link to the RFI form. The withdrawal will sit in <mark style="color:orange;">`REVIEWING`</mark> until you respond to the RFI via the link in the email and a compliance analyst marks it satisfactory.

You can simulate the behaviour by including `SEND_RFI` text into the `externalReference` For example: `"testing SEND_RFI attempt 3"`.  The withdrawal remains in <mark style="color:orange;">`REVIEWING`</mark> until you respond to the RFI. At that point, we will review your submission and provide feedback, and then manually push the transaction forward. The primary goal of this test is to familiarize your team with our RFI process and address any questions beforehand, ensuring a smooth transition to production.

{% hint style="info" %}
Requires at least one active member with the `compliance` access role on your account. Otherwise no RFI is created, the withdrawal fails AML and gets cancelled in accordance with the `FAIL_AML` pattern.
{% endhint %}

#### 5. Recipient bank rejects the money

A common scenario in the Australian domestic payment system involves a transaction that appears successful initially, but is later reversed by the recipient’s bank—sometimes days after processing. This can happen for various reasons, such as an invalid or closed account number, an account in the wrong currency, or other account-related issues.

You can simulate the behaviour. Your `externalReference` must include this text: `FAIL_ACC`. For example: `"testing FAIL_ACC attempt 8"`. The withdrawal will be completed, and next moment failed and refunded.
