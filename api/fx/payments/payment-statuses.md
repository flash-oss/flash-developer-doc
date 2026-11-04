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

The status of a payment can be any of the following:

Payment Statuses:

* INITIALISING – a draft payment. It may or may not have all required information to be executed. Typically, this never happens through the API. Can be created via the [Flash Payments app](http://app.uat.flash-payments.com.au/) under certain circumstances.
* OPEN – The payment is in progress.
* CLOSED – The payment was successfully delivered.
* FAILED – An error occurred during processing.
* CANCELLED – The payment did not complete and was cancelled, usually by the Flash Payments operations team.

{% hint style="info" %}
Important to understand that the `Payment.history` is a transfer logs and is not related to payment status at all. Sometimes the history may contain half a dozen entries, however your payment will go through only two steps: `OPEN` and `CLOSED`.
{% endhint %}
