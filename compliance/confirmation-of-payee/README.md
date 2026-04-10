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

### Making a CoP Request

To verify account details, execute the `confirmationOfPayee` mutation. You must provide the `accountIdType`, the relevant account details, and the `accountName` you wish to verify.

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
```javascript
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
```javascript
{
  "data": {
    "confirmationOfPayee": {
      "success": true,
      "code": "MATCH",
      "message": "Account name matches",
      "billable": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

The `code` field in the response indicates the [result of the verification](https://developer.flash-payments.com/~/revisions/QODtCCHQ0ULYQp2U9s5d/other/confirmation-of-payee/response-codes).&#x20;

The `billable` field indicates whether the CoP request may be subject to a fee.&#x20;

{% hint style="info" %}
CoP requests are billed based on your agreement with Flash Payments. Contact Flash Payments support for details on pricing.
{% endhint %}
