---
description: History of changes to this API schema
---

# Change log

## 2025-07-25

### Added

The new optional field called `reason` was added to the `createWithdrawal`  mutation and `Withdrawal`  type.

If you have a payment/transaction/payout **purpose** - you must submit it to us via the API. See all the possible purpose codes (aka reason values) in the [GraphQL playground](https://api.flash-payments.com) (click the link in the header of this website), look inside the "docs" by the word "reason". You need to find the GraphQL enum called `WithdrawalReason`.

## 2025-07-24

### Added

An optional `affiliation` field has been added to the `login` mutation.\
This field accepts the `Affiliation` enum values `FP_AUS` or `FP_LUX`, allowing users to specify which Flash Payments subsidiary account to access when multiple contractual agreements exist. It is only required if your have more than one such agreement with us.

## 2025-05-02

### Removed (BREAKING)

Removed `depositDetails` query as deprecated and non-functioning since March 2024.

## 2025-01-24

### Changed

Improved validation rules for the following mutations: [createSender](senders.md#create-a-sender), [updateSender](senders.md#update-sender), [createInstitution](institutions.md#creating-institutions), [createSubClient](sub-clients.md#creating-a-sub-client), [createWithdrawal](withdrawals/withdraw-funds.md), and [createPayment](payments/send-funds.md)\
Affected fields: `idDoc.docNumber` `idDoc.issuer` `legalName` `businessNumber` `legalName` `externalId`. These fields allow only the ASCII characters now.

Also, the first, last, and middle names are limited to 75 chars now.

Also, FQDN-like names will be rejected. Examples: "Aaron x.com", "Ben.eu", "Visit as at URL:example.com/about", etc.

## 2025-01-18

### Added

New fields have been added to the [createSender](senders.md#create-a-sender), [updateSender](senders.md#update-sender), and [createSubClient](sub-clients.md#creating-a-sub-client) mutations. You can now provide `idDoc.country`, `idDoc.issueDate`, and `idDoc.expiryDate`. The `idDoc.issuer` field now only stores additional information about the issuer, while `idDoc.country` holds the country code of the country that issued the document.\
To avoid creating a breaking change, we currently allow you to provide `idDoc.country` and/or `idDoc.issuer`. In the future, we plan to make `idDoc.country` a **required** field.

## 2024-12-09

### Added

New [updateSubClient](sub-clients.md#update-sub-clients) mutation. You can change a sub-client's `externalId` now, but nothing else.

## 2024-10-22

### Added

New the [Conversions](conversions/) feature.

* New mutation `createConversion`. New queries `conversion` and `conversions`.
* The `FromCurrency` enum used to have only one item `AUD`. Now there are 8: `AUD CAD CHF EUR GBP NZD SGD USD`.
* New property for the `Quote` type: `applicability`. It indicates if the quote can be used for _payments_ or _conversions_.

## 2024-10-11

### Fixed

The `bankInfo` [query](https://developer.flash-payments.com/reference-data/bank-information) was not returning information about BICs and IBANs. It works now.

## 2024-10-01

### Changed (BREAKING)

Improved validation of `recipient`, `sender`, `withdrawal`, `institution` related mutations. From now on `email`, `companyName`, `legalName`, `businessNumber`, `externalReference` must include only the ASCII characters.

## 2024-05-18

### Removed (BREAKING)

Removed `AccountIdType.PH_CASH`, `Recipient.phCashoutNetwork` with `RecipientInput.phCashoutNetwork` and the corresponding enum type `PhCashoutNetwork` from the GraphQL schema. These were not working for more than a year.

## 2024-05-17

### Removed (BREAKING)

Removed `AccountIdType.RIPPLE`, `Recipient.rippleAddress` with `Recipient.destTag` from the GraphQL schema. These were deprecated 2.5 years ago.

## 2024-04-16

### Removed (BREAKING)

Removed `Sender.isRipple` and `CurrencyIso3.XRP` from the GraphQL schema. These were deprecated 2.5 years ago.

## 2024-03-13

### Added

New [static codes and standard status messages](https://developer.flash-payments.com/reference-data/rejection-codes) introduced for rejected (aka cancelled) withdrawals. When withdrawals [get cancelled](https://developer.flash-payments.com/withdrawals/withdrawal-statuses) you can clearly see why using the new `rejectCode` and `statusMessage`. Available in the `Withdrawal` type and sent to your application via the [withdrawal\_cancelled](https://developer.flash-payments.com/webhooks#withdrawal_cancelled) webhook.

## 2024-02-05

### Changed

Improved [`updateRecipient`](https://developer.flash-payments.com/recipients#update-recipient) to respond with appropriate error message when trying to change the recipient's `accountIdType` which is not allowed by design.

## 2023-11-01

### Added

Added [`instructingInstitution`](withdrawals/withdraw-funds.md#instructing-institutions) to the `CreateWithdrawalInput.`

## 2023-10-25

### Removed (BREAKING)

* Fields `acceptingInstructionInstitutionSenderId` and `acceptingMoneyInstitutionSenderId` were removed from the `CreateWithdrawalInput`.

### Added

* Instead `instructingInstitutionId` was added. (A new API for creating "institutions" is coming soon, but at the moment they can be created via the Flash Connect.)

## 2023-08-22

### Added

* [`statement`](balance/statement.md) query.
  * This query returns the same data as the Download CSV button on the Account Statement page of the [Flash Connect](https://connect.flash-payments.com). It explains every change of you primary balance.
  * Currently it returns exactly 1 day of data. We plan to make date range selection more flexible in the future.

## 2023-08-04

### Changes

* Changed links from [flash-fx.com](https://flash-fx.com/) to [flash-payments.com](https://flash-payments.com/) domain. Old domain will continue working unit future notice.

## 2023-07-28

### Changes

* Our system always allowed `accountNo` to have letter. However, our API forced digits only. So, from now on, when you use `createRecipient`, your `accountNo` can have both letters and digits.

## 2023-05-25

### Changes

* Improved mobile phone validation. Now if mobile starts with "00" it's treated as if it starts with "+".
* Quote size accepts positive numbers only.

### Changes (BREAKING)

* The `recipient.mobile` and `sender.mobile` can accept only **valid** international phone numbers.

## 2023-03-15

### Added

* Added the `withdrawal_pending` [webhook](webhooks/). Invoked after the transaction is sent to the recipient bank for processing.
* Added [example](https://developer.flash-fx.com/webhooks#withdrawal_pending) for the `withdrawal_pending` [webhook](webhooks/).

## 2023-03-03

### Added

* `idempotencyKey` to `createPayment` input.
* `idempotencyKey` to `createWithdrawal` input.
* New [sub-client](sub-clients.md) status - `UNAPPROVED`.

## 2023-02-23

### Added

* Fixed the `withdrawal_reviewing` [webhook](webhooks/). It was never sent before even though declared on the Flash Connect website.
* Added [example](https://developer.flash-fx.com/webhooks#withdrawal_reviewing) for the `withdrawal_reviewing` [webhook](webhooks/).

## 2023-02-08

### Removed (BREAKING)

* Removed the auto-creation of `id` for embedded senders and recipients. This means that if you (or FlashFX system) have created withdrawals and payments without explicitly providing sender or recipient ID then from now on the `sender.id` or `recipient.id` will be `null`. But, if you create payments or withdrawals via this API then you will always have `payment.sender.id` or `withdrwal.recipient.id`.

## 2022-09-06

### Added

* Missing `expireAt` property to the `Quote` object returned by the `quote` query.

## 2022-09-02

### Added

* New `REVIEWING` status to deposit and withdrawal status enum.
  * _`REVIEWING` : deposit_/withdrawal _is being internally checked by our compliance team before proceeding._
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
    * `accountName`
    * `accountNo`
    * `bsb`
* New item in `deposit.sender:`
  * `bankName`
* New mutation to refund deposits:
  * `refundDeposit(id:ID! input:RefundDepositInput): RefundDepositReply`

## 2022-04-22

### Changes

* Additional validation for `dob` field introduced for Senders, Recipients and Sub-clients to enforce the data is entered in YYYY-MM-DD format. The field will also allow for only the data after 1900-01-01 and individuals of 18 years of age or older.

## 2022-04-12

### Changes

* Added validation of address fields. From now on, any address submitted as a part of any transaction should only include ASCII characters.

## 2022-02-25

### Changes

* Added `deposit.sender.accountName` so that you can query who deposited money to your Virtual Account Number (VAN).

## 2022-02-14

### Changes

* Additional validation for `lastName`, `middleName` and `firstName` introduced allowing only for latin alphabetical characters and special symbols: <img src=".gitbook/assets/Screenshot 2022-02-14 at 14.51.52.png" alt="" data-size="line">

## 2022-02-09

### Changes

* `FundingAccount` type has been extended to include all deposit details you need to bring money to Australia. Your account address can now be retrieved using `accountAddress` property along with `name` and `address` fields which identify the accepting financial institution associated with your account.

## 2022-01-12

### Changes

* `lastName` and `firstName` fields of `Sender` and `Recipient` objects can now be one symbol long to allow for initials. Please note that such single symbols have to be alphabetic.

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

* The `totalFee` property to both `Deposit` and `Withdrawal` types as well as webhook payloads.

## 2021-06-22

### Added

* The `bankInfo` reference query. You can now validate your BSB for existence, check if we support your BIC, and retrieve BIC (aka SWIFT code) by IBAN.

## 2021-06-17

### Added

* The `fundingAccounts` and `SubClient.fundingAccounts` queries. It returns international bank account numbers you can deposit in order to bring money to Australia. See [Auto receive funds](payments/auto-receive-funds.md) for more details.

## 2021-05-25

### Changed

* The `senderId` was always required when creating withdrawals via `createWithdrawal`. But now, if you provided the `subClientId` and didn't provide the `senderId` the sub-client becomes the sender, and will be reported to the government as the sender. However, you still must provide the `senderId` if your sub-client moves funds for other people/companies.

## 2021-05-17

### Changed (BREAKING)

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
* **Introduced sub-client feature** – transactional virtual account numbers for AUD processing in Australia.
  * query `subClients(input: SubClientQueryInput): [SubClient]`
  * query `subClient(id: ID!): SubClient`
  * mutation `createSubClient(input: CreateSubClientInput!): MutateSubClientReply`
* Deposit and withdrawal webhooks now include `subClient` object with sub-client information when present

## 2021-01-07

### Removed (BREAKING)

These types and fields were never used by anyone for couple of years.

* Removed enum `DepositMechanism`.
* `PaymentInput` and `ConfirmPaymentInput` fields:
  * removed `depositMechanism`
  * removed `depositReference`
  * removed `depositAmount`

## 2020-12-16

### Added

* New item in `BsbDepositDetails`
  * `accountName` - Australian account name.
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

### Changed (BREAKING)

* Fixed typo in `RecipientQueryInput` field name. `snaps` -> `cnaps`

## 2020-05-04

### Added

* New compliance-related fields to the `CreateWithdrawalInput` input:
  * `acceptingMoneyInstitutionSenderId` - you must pre-create this `Sender` and submit every time if you are not the FI who collected the money for this withdrawal.
  * `acceptingInstructionInstitutionSenderId` - you must pre-create this `Sender` and submit every time if you were instructed by other FI to make this withdrawal.

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
* You can now query your recipients by `accountIdType` property. For example: ` recipients(input: {`` `` `**`accountIdType: PH_CASH`**` `` ``}) `

### Removed (BREAKING)

* Removed the unused enum `PaymentType`. It has no sense and was deprecated a year ago.
  * Simultaneously removed properties `Payment.paymentType`, `PaymentQueryInput.paymentTypes`, `PaymentInput.paymentType`.
* Removed the long deprecated `PaymentInput.recipient` object. The only way to provide a recipient for a payment is via `PaymentInput.recipientId`. You would need to [pre-create the recipient](recipients/#create-a-recipient) beforehand.
* Removed the never used `AccountIdType` enum values: `BPAY`, `FIN_BTN`, `INTERAC`.
