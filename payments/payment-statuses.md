# Payment statuses

The GraphQL schema defines statuses like this:

```graphql
enum PaymentStatus {
  INITIALISING
  OPEN
  CLOSED
  FAILED
  CANCELLED
}
```

The current status of your payment.

* INITIALISING - a draft payment. It may or may not have all required information to be executed.
  * Typically never happens via API. Can be created via the [Flash Payments app](https://app.flash-payments.com/) under certain circumstances.
* OPEN - the payment is in **progress**.
* CLOSED - means **successfully** delivered.
* FAILED - an **error** has occurred with this payment.
* CANCELLED - the payment has not completed and has been cancelled.
  * Usually done by Flash Payments operations team.

{% hint style="info" %}
Important to understand that the `Payment.history` is a transfer logs and is not related to payment status at all. Sometimes the history may contain half a dozen entries, however your payment will go through only two steps: `OPEN` and `CLOSED`.
{% endhint %}
