---
description: Verify that a payee's account name matches their bank account details
---

# Confirmation of Payee

Confirmation of Payee (CoP) is a security feature that verifies whether an account holder's name matches the account details you provide. It helps to:

* Reduce fraud by verifying the account holder's name before sending funds
* Prevent accidental payments to wrong accounts due to mistyped details
* Provide confidence that money is being sent to the intended recipient
* Alert you when the account name doesn't match or only partially matches

{% hint style="info" %}
The feature is also available through the [Flash Connect](https://connect.flash-payments.com/) portal. You can perform CoP checks directly from the UI, view your request history, and monitor usage statistics without writing any code. This can be useful for manual verification workflows or for getting familiar with the feature before integrating it into your application via the API.
{% endhint %}

### Before you start

Two things must be in place before the mutation returns a result: Flash Payments team has enabled Confirmation of Payee over the API for your account, and your company account administrator has accepted the Confirmation of Payee disclaimer in Flash Connect. The disclaimer is shown on the Confirmation of Payee page there and is accepted once per account. Until then the mutation replies with `success: false` and either `code: "COP_NOT_AVAILABLE"` (not enabled) or `code: "COP_API_DISABLED"` (disclaimer not accepted).

### Making a CoP request

To verify account details, execute the `confirmationOfPayee` mutation. Only `accountIdType: BSB` is supported today: provide the `bsb`, the `accountNo` and the `accountName` you wish to verify. Any other `accountIdType` replies with `code: "ACCOUNT_TYPE_NOT_SUPPORTED"`.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      accountIdType: "BSB",
      bsb: "012003",
      accountNo: "123456789",
      accountName: "John Smith",
    },
  },
query: `
  mutation ($input: CopInput!) {
    confirmationOfPayee(input: $input) {
      success code message billable
    }
  }`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation ($input: CopInput!) {
  confirmationOfPayee(input: $input) {
    success
    code
    message
    billable
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
 {
  "input": {
    "accountIdType": "BSB",
    "bsb": "012003",
    "accountNo": "123456789",
    "accountName": "John Smith"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "confirmationOfPayee": {
      "success": true,
      "code": "MATCH",
      "message": "Account name matches the account name entered",
      "billable": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

The `code` field in the response indicates the [result of the verification](cop-response-codes.md).&#x20;

The `billable` field indicates whether the CoP request may be subject to a fee.

The `success` field is `false` when the check was not carried out, for example when CoP is not enabled for your company or a limit was reached. `NOT_MATCH`, `ACCOUNT_CLOSED` and `ACCOUNT_NOT_FOUND` are results and come with `success: true`, so act on `code`.

{% hint style="info" %}
CoP requests are billed based on your agreement with Flash Payments. Contact Flash Payments support for details on pricing.
{% endhint %}
