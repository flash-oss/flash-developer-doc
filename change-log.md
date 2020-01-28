---
description: History of changes to this API schema
---

# Change log

## 2020-01-28

### Added

* Introduced Withdrawals - send money from your account \(FlashFX digital wallet\) to local banks.
  * query `withdrawal(id: ID): Withdrawal`
  * query `withdrawals(input: WithdrawalQueryInput): [Withdrawal]`
  * mutation `createWithdrawal(input: CreateWithdrawalInput!): CreateWithdrawalReply`
* `PHP` currency support.
* `middleName` property in recipients and senders.
* `PAYID` recipient type for Australian local payments or withdrawals.
  * `Recipient.payid` new property.
* `PH_CASH` recipient type for Philippines cash network payments.
  * `Recipient.phCashoutNetwork` new property.
* You can now query your recipients by `accountIdType` property. For example: `recipients(input: {` **`accountIdType: PH_CASH`** `})`

### Removed \(BREAKING\)

* Removed the unused enum `PaymentType`. It has no sense and was deprecated a year ago.
  * Simultaneously removed properties `Payment.paymentType`, `PaymentQueryInput.paymentTypes`, `PaymentInput.paymentType`.
* Removed the long deprecated `PaymentInput.recipient` object. The only way to provide a recipient for a payment is via `PaymentInput.recipientId`. You would need to [pre-create the recipient](recipients/#create-a-recipient) beforehand.
* Removed never used `AccountIdType` enum values: `BPAY`, `FIN_BTN`, `INTERAC`.



