---
description: History of changes to this API schema
---

# Change log

## 2023-02-24

### Added

* Fixed the `withdrawal_reviewing` [webhook](webhooks/). It was never sent before even though declared on the Flash Connect website.&#x20;
* Added [example](https://developer.flash-fx.com/webhooks#withdrawal\_reviewing) for the `withdrawal_reviewing` [webhook](webhooks/).

## 2023-02-08

### Removed (BREAKING)

* Removed the auto-creation of `id` for embedded senders and recipients. This means that if you (or FlashFX system) have created withdrawals and payments without explicitly providing sender or recipient ID then from now on the `sender.id` or `recipient.id` will be `null`. But, if you create payments or withdrawals via this API then you will always have `payment.sender.id` or `withdrwal.recipient.id`.

## 2022-09-06

### Added

* Missing `expireAt` property to the `Quote` object returned by the `quote` query.

## 2022-09-02

### Added

* New `REVIEWING` status to deposit and withdrawal status enum.
  * _`REVIEWING` : deposit_/withdrawal _is being **manually** checked (e.g. compliance) before proceeding._
  * Added corresponding [webhook](webhooks/) event types: `deposit_reviewing` and `withdrawal_reviewing`.

## 2022-08-25

### Added

* New [webhook](webhooks/) event type: `deposit_initiated` to notify that we received a deposit but not yet cleared it.
  * Previously only the `deposit_cleared` was sent and customers had no idea we are holding (reviewing) the deposit.
  * The `deposit_cleared` will be sent immediately as we approve (clear) the deposits. So, no changes here.

## 2022-07-01

### Added

* New item in `deposit` and related webhooks:
  * `recipient` - deposit recipient information as specified by deposit sender for this transaction:
    * `accountName` &#x20;
    * `accountNo`&#x20;
    * `bsb`&#x20;
* New item in `deposit.sender:`
  * `bankName`&#x20;
* New mutation to refund deposits:
  * `refundDeposit(id:ID! input:RefundDepositInput): RefundDepositReply`

## 2022-04-22

### Changes

* Additional validation for `dob` field introduced for Senders, Recipients and Sub-clients to enforce the data is entered in YYYY-MM-DD format. The field will also allow for only the data after 1900-01-01 and individuals of 18 years of age or older.

## 2022-04-12

### Changes

* Added validation of address fields. From now on, any address submitted as a part of any  transaction should only include ASCII characters.

## 2022-02-25

### Changes

* Added `deposit.sender.accountName` so that you can query who deposited money to your virtual bank account.

## 2022-02-14

### Changes

* Additional validation for `lastName`, `middleName` and `firstName` introduced allowing only for latin alphabetical characters and special symbols: <img src=".gitbook/assets/Screenshot 2022-02-14 at 14.51.52.png" alt="" data-size="line">

## 2022-02-09

### Changes

* `FundingAccount` type has been extended to include all deposit details you need to bring money to Australia. Your account address can now be retrieved using `accountAddress` property along with `name` and `address` fields which identify the accepting financial institution associated with your bank account.

## 2022-01-12

### Changes

* &#x20;`lastName` and `firstName` fields of `Sender` and `Recipient` objects can now be one symbol long to allow for initials. Please note that such single symbols have to be alphabetic.

## 2021-11-02

### Changes

* Occasionally `Payment` objects do not have `sender` or `recipient` properties. Thus these properties are now marked at "not required" (exclamation mark was removed) when querying payments.
* We have added rate limiting. You can receive HTTP 429 error code and get temporary blocked if abusing the API too much.

## 2021-08-13

### Changed (BREAKING)

* Removed **XRP** currency form the list of supported currencies.

## 2021-07-22

### Changed (BREAKING)

* While creating sub-clients the address of the person/company was not required. It was a bug which was fixed. To create a sub-client you would also need their: **street** address, **suburb**/city, **state**/region, **postcode**, and country.

## 2021-07-14

### Changes

* Allow `accountNo` to be 4 digits long. Some old Japanese bank accounts could be just 4 digits.

## 2021-07-08

### Added

* The ability to query deposits, withdrawals, payments by the associated sub-client (`subClientId`).
* The payments can also have sub-clients now. Added the `Payment.subClient` field.

## 2021-06-29

### Added

* &#x20;The `totalFee` property to both `Deposit` and `Withdrawal` types as well as webhook payloads.

## 2021-06-22

### Added

* &#x20;The `bankInfo` reference query. You can now validate your BSB for existence, check if we support your BIC, and retrieve BIC (aka SWIFT code) by IBAN.

## 2021-06-17

### Added

* &#x20;The `fundingAccounts` and `SubClient.fundingAccounts` queries. It returns international bank account numbers you can deposit in order to bring money to Australia. See [Auto receive funds](payments/auto-receive-funds.md) for more details.

## 2021-05-25

### Changed

* &#x20;The `senderId` was always required when creating withdrawals via `createWithdrawal`. But now, if you provided the `subClientId` and didn't provide the `senderId` the sub-client becomes the sender, and will be reported to the government as the sender. However, you still must provide the `senderId` if your sub-client moves funds for other people/companies.

## 2021-05-17

### Changed (BREAKING)

* Replaced `docIssuer`, `docType` and `docNumber` fields from `CreateSubClientInput` with `idDoc` nested field instead.&#x20;

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
* **Introduced sub-client feature** – transactional virtual account numbers for AUD processing in Australia.
  * query `subClients(input: SubClientQueryInput): [SubClient]`
  * query `subClient(id: ID!): SubClient`
  * mutation  `createSubClient(input: CreateSubClientInput!): MutateSubClientReply`
* Deposit and withdrawal webhooks now include `subClient` object with sub-client information when present

## 2021-01-07

### Removed (BREAKING)

These types and fields were never used by anyone for couple of years.

* Removed enum `DepositMechanism`.
* `PaymentInput` and `ConfirmPaymentInput` fields:
  * removed `depositMechanism`&#x20;
  * removed `depositReference`&#x20;
  * removed `depositAmount`&#x20;

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
  * `legalName`&#x20;
  * `tradingAsName`
  * `businessNumber` - differs by country, e.g. ABN in Australia.
  * `acn` - Australian Company Number. Should not be used for other countries.

## 2020-06-18

### Changed (BREAKING)

* Fixed typo in `RecipientQueryInput` field name. `snaps` -> `cnaps`&#x20;

## 2020-03-05

### Changed (BREAKING)

* Fixed typo in the `WithdrawalStatus` enum item. `INITIALISING` -> `INITIALISED`

## 2020-02-12

### Added

* `callbackUri` to `Payment`, `Withdrawal`, `CreateWithdrawalInput`. Now you can query the callback/webhook URI you have supplied earlier. Also, this allows you to receive webhooks when you create a withdrawal (aka _local payout_).

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

* Introduced Withdrawals - send money from your account (FlashFX digital wallet) to local banks.
  * query `withdrawal(id: ID): Withdrawal`
  * query `withdrawals(input: WithdrawalQueryInput): [Withdrawal]`
  * mutation `createWithdrawal(input: CreateWithdrawalInput!): CreateWithdrawalReply`
* `PHP` currency support.
* `middleName` property in recipients and senders.
* `PAYID` recipient type for Australian local payments or withdrawals.
  * `Recipient.payid` new property.
* `PH_CASH` recipient type for Philippines cash network payments.
  * `Recipient.phCashoutNetwork` new property.
* You can now query your recipients by `accountIdType` property. For example: `recipients(input: {`` `**`accountIdType: PH_CASH`**` ``})`

### Removed (BREAKING)

* Removed the unused enum `PaymentType`. It has no sense and was deprecated a year ago.
  * Simultaneously removed properties `Payment.paymentType`, `PaymentQueryInput.paymentTypes`, `PaymentInput.paymentType`.
* Removed the long deprecated `PaymentInput.recipient` object. The only way to provide a recipient for a payment is via `PaymentInput.recipientId`. You would need to [pre-create the recipient](recipients/#create-a-recipient) beforehand.
* Removed the never used `AccountIdType` enum values: `BPAY`, `FIN_BTN`, `INTERAC`.

