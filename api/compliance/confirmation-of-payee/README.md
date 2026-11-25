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

### Which accounts you can check

Confirmation of Payee works for individual accounts, joint accounts and business accounts. The request is the same for all three: the **BSB**, the **account number** and the **name** you expect to be on the account. You do not send the account type, and the reply does not tell you the account type.

* **Individual account.** Send the person's name.
* **Joint account.** Send the name of one account holder. It is compared with each account holder the bank lists, so you do not need to send them all.
* **Company account.** Send the legal name. Endings such as "Pty Ltd" and "Limited" make no difference. A trading name matches only if the bank holds it for the account.

### How the name is compared

When the account is found, the reply has one of three codes. Letter case, punctuation and titles such as "Mr" make no difference.

| Code          | What it means                                    | Examples                                                                                                                                                                                      |
| ------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `MATCH`       | The name is the same as the name on the account. | <ul><li>"John Smith" for an account held by John Smith. </li><li>"Jane Smith" for a joint account of John Smith and Jane Smith. </li><li>"Acme" or "Acme Limited" for Acme Pty Ltd.</li></ul> |
| `CLOSE_MATCH` | The name is nearly the same.                     | <ul><li>"Jon Smith", "J Smith" or "Smith John" for John Smith. </li><li>"John Smith" for John Andrew Smith. </li><li>"Acme" for Acme Holdings Pty Ltd.</li></ul>                              |
| `NOT_MATCH`   | The name is different.                           | <ul><li>"John Williams" for John Smith. </li><li>"Peter Smith" for a joint account of John Smith and Jane Smith. </li><li>"John Smith" for Acme Pty Ltd.</li></ul>                            |

The examples show the usual result. The result depends on how the bank holds the name, so the same payee can get `MATCH` at one bank and `CLOSE_MATCH` at another. The reply never includes the actual name the bank holds.

What to do for each code is on the [CoP response codes](cop-response-codes.md) page. To try each account type in UAT, use the keywords on the [Testing CoP integration](testing-cop-integration.md) page.

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
