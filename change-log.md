---
description: History of changes to this API schema
---

# Change log

## 2021-05-17

### Changed \(BREAKING\)

* Replaced `docIssuer`, `docType` and `docNumber` fields from `CreateSubClientInput` with `idDoc` nested field instead. 

## 2021-04-26

### Changed

* `RecipientAccountIdType` was fully duplicating the `AccountIdType`. Replace the former with the latter. This might break your auto-generated code in strongly typed languages. But won't change any API queries or responses. So, this change is not considered to be a breaking.

## 2021-04-16

### Added

* Added ability to disable and activate sub-clients
  * mutation `disableSubClient(id: ID!): MutateSubClientReply`
  * mutation `activateSubClient(id: ID!): MutateSubClientReply`

## 2021-04-12

### Added

* Added deposit queries
  * query `deposits(input: DepositQueryInput): [Deposit]`
  * query `deposit(id: ID!): Deposit`
* Introduced sub-client feature  – transactional accounts for AUD processing in Australia.
  * query `subClients(input: SubClientQueryInput): [SubClient]`
  * query `subClient(id: ID!): SubClient`
  * mutation  `createSubClient(input: CreateSubClientInput!): MutateSubClientReply`
* Deposit and withdrawal webhooks now include `subClient` object with sub-client information when present

## 2021-01-07

### Removed \(BREAKING\)

These types and fields were never used by anyone for couple of years.

* Removed enum `DepositMechanism`.
* `PaymentInput` and `ConfirmPaymentInput` fields:
  * removed `depositMechanism` 
  * removed `depositReference` 
  * removed `depositAmount` 

## 2020-12-16

### Added

* New item in `BsbDepositDetails`
  * `accountName` - Australian bank account name.
* New item in `Withdrawal`
  * `statusMessage` - a human readable message of the current status reason, like processing error messages.
* New item in `Payment` and `PaymentInput`
  * `sourceOfFunds` - mandatory field for some destinations.
* New enum `SourceOfFunds`.

## 2020-10-14

### Added

* New items in both query type and mutation input `Senders`.
  * `legalName` 
  * `tradingAsName`
  * `businessNumber` - differs by country, e.g. ABN in Australia.
  * `acn` - Australian Company Number. Should not be used for other countries.

## 2020-06-18

### Changed \(BREAKING\)

* Fixed typo in `RecipientQueryInput` field name. `snaps` -&gt; `cnaps` 

## 2020-03-05

### Changed \(BREAKING\)

* Fixed typo in the `WithdrawalStatus` enum item. `INITIALISING` -&gt; `INITIALISED`

## 2020-02-12

### Added

* `callbackUri` to `Payment`, `Withdrawal`, `CreateWithdrawalInput`. Now you can query the callback/webhook URI you have supplied earlier. Also, this allows you to receive webhooks when you create a withdrawal \(aka _local payout_\).

## 2020-02-11

### Added

* New items in `RecipientQueryInput`. This means that recipients can be searched by:
  * `firstName`
  * `lastName`
  * `middleName`
  * `dob` - date of birth
  * `companyName`
  * `phCashoutNetwork`
  * `payid`
  * `bic`
  * `iban`
  * `aba`
  * `bsb`
  * `clabe`
  * `cnaps`
  * `sortCode`
  * `ifsc`
  * `accountNo`
  * `rippleAddress`
  * `externalId` - ID in your system

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



