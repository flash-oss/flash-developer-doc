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
  * Typically never happens via API. Can be created via the [FlashFX app](https://app.flash-fx.com) under certain circumstances.
* OPEN - the payment is in **progress**.
* CLOSED - means **successfully** delivered.
* FAILED - an **error** has occurred with this payment.
* CANCELLED - the payment has not completed and has been cancelled.
  * Usually done by FlashFX operations team.



