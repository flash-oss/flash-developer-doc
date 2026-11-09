# Testing CoP integration

In the UAT environment, you can simulate different CoP responses by using specific substring keywords in the `accountName` field and any valid `bsb` and `accountNo` values.

{% hint style="info" %}
The pattern matching on the  `accountName` field is **substring-based** and **case-insensitive,** e.g. using `"Test Copimatch"`, `"John copimatch Smith"`, or `"COPIMATCH"` will all trigger the same scenario.
{% endhint %}

Here is a current list of the testing keywords and the most common [response codes](cop-response-codes.md). Please note this list **can be changed over time.**

| Account Name Keyword | Response Code        | Simulated Account Type     |
| -------------------- | -------------------- | -------------------------- |
| `copimatch`          | `MATCH`              | Individual                 |
| `copijmatch`         | `MATCH`              | Individual (joint account) |
| `copcmatch`          | `MATCH`              | Company                    |
| `copiclosematch`     | `CLOSE_MATCH`        | Individual                 |
| `copijclosematch`    | `CLOSE_MATCH`        | Individual (joint account) |
| `copcclosematch`     | `CLOSE_MATCH`        | Company                    |
| `copinotmatch`       | `NOT_MATCH`          | Individual                 |
| `copcnotmatch`       | `NOT_MATCH`          | Company                    |
| `copclosed`          | `ACCOUNT_CLOSED`     | -                          |
| `copnotfound`        | `ACCOUNT_NOT_FOUND`  | -                          |
| `coperror`           | `COP_PLATFORM_ERROR` | -                          |

{% hint style="info" %}
You can start with testing CoP requests in the [API Playground](https://api.uat.flash-payments.com.au/). Log in with your UAT credentials and use the [`confirmationOfPayee` mutation](./#making-a-cop-request).
{% endhint %}

### Integration Examples

#### CoP as a standalone investigation tool

CoP can be used independently from your payment flows as an account investigation tool. For example, you may want to verify account holder details during onboarding, reconciliation, or dispute resolution, without initiating a payment. A typical investigation flow looks like this:

1. Collect the account details you wish to verify: BSB, account number, and the expected account holder name.
2. Call `confirmationOfPayee` with those details.
3. Inspect the `code` in the response.
4. If `MATCH` then the account holder name is confirmed. Record the result for your records.
5. If `CLOSE_MATCH` then the name partially matches. The reply does not include the name the bank holds, so ask your user to confirm the name they entered.
6. If `NOT_MATCH` then the name does not match the account. This may warrant further due diligence.
7. If `ACCOUNT_CLOSED` then the account is no longer active. Flag it accordingly.
8. If `ACCOUNT_NOT_FOUND` then no CoP record exists for these details: they are wrong, or the account is not covered by CoP. Check the details with your payee.
9. If `COP_PLATFORM_ERROR` then the check could not be completed. The account may have opted out of CoP. Retry later or use alternative verification methods.

{% hint style="info" %}
Each CoP request is logged and available in your [Flash Connect](https://connect.flash-payments.com/) request history, making it easy to maintain an audit trail of your verification checks.
{% endhint %}

#### CoP as a part of your payment integration flow

{% hint style="warning" %}
While CoP is fully functional on our platform, and the adoption among Australian financial institutions is growing, the **Australian CoP coverage is currently limited.**&#x20;

This means a significant portion of accounts may return `ACCOUNT_NOT_FOUND` or `COP_PLATFORM_ERROR`  not because the details are wrong, but because the receiving institution does not yet fully participate in CoP.

Additionally, financial institutions store account holder names in varying formats (e.g., abbreviated names, reordered words, special characters). The current matching algorithm used by the Australian Payments Plus (AP+) relies on basic string similarity, which can produce false-negative `NOT_MATCH` results for accounts that are, in fact, correct.

For this reason, **we currently recommend using CoP as a standalone verification tool** rather than as a blocking step in your payment flow.
{% endhint %}

A typical future integration flow could look like this:

1. Before submitting a payment or withdrawal, call `confirmationOfPayee` with the recipient's account details.
2. Inspect the `code` in the response.
3. If `MATCH` then proceed with the payment.
4. If `CLOSE_MATCH` then display a warning to your user and let them decide whether to proceed.
5. If `NOT_MATCH` then alert the user and recommend they verify the account details.
6. If `ACCOUNT_CLOSED` then block the payment and ask the user for other account details.
7. If `ACCOUNT_NOT_FOUND` then treat the account as unverified: the details may be wrong, or the account may not be covered by CoP. Ask the user to check the details and decide according to your own risk rules.
8. If `COP_PLATFORM_ERROR` then optionally proceed, as the account may have opted out of CoP.

{% hint style="info" %}
**CoP is advisory**. The response does not block payments automatically. It is **your responsibility** to act on the result codes appropriately for your use case.
{% endhint %}

#### Error Handling&#x20;

If Confirmation of Payee is not enabled for your company over the API, the mutation replies with `success: false` and `code: "COP_NOT_AVAILABLE"`. Contact Flash Payments support to enable it.

If your company has not yet accepted the Confirmation of Payee disclaimer in Flash Connect, the reply is:

```json
{
  "data": {
    "confirmationOfPayee": {
      "success": false,
      "code": "COP_API_DISABLED",
      "message": "This service's disclaimer has not been accepted. Open Flash Connect to review and accept it.",
      "billable": null
    }
  }
}
```

Open the Confirmation of Payee page in Flash Connect and accept the disclaimer. One acceptance covers the whole company.

Standard [rate limiting](../../other/rate-limiting.md) applies to CoP requests. CoP also has two limits of its own. Both reply with `code: "COP_LIMIT_EXCEEDED"`:

* A free monthly allowance of requests, if your company has no CoP pricing plan. The `message` says how many requests you have made this month. The allowance resets on the first day of the next month (UTC). Contact Flash Payments support for a pricing plan.
* A protection against many requests ending in `NOT_MATCH` or `ACCOUNT_NOT_FOUND` within a short time. It lifts by itself. Slow down and retry later.

These limits apply in UAT as well: `copinotmatch`, `copcnotmatch` and `copnotfound` count as failed lookups, so a test loop over them ends in `COP_LIMIT_EXCEEDED` for a while.

{% hint style="info" %}
When the [response code](cop-response-codes.md) is `COP_PLATFORM_ERROR`, it may indicate that the account holder has opted out of CoP verification. The general recommendation is to proceed with the payment.
{% endhint %}

