# Conversion statuses

The GraphQL schema defines statuses like this:

```graphql
enum ConversionStatus {
    INITIALISED
    PENDING
    CONVERTED
    FAILED
    CANCELLED
}
```

The current status of your conversion.

* INITIALISED - a conversion was created but nothing else happened to it.
  * Typically never happens via API.
* PENDING - the conversion is in **progress**. The typical status of the conversion after you create it.
* CONVERTED - means **successfully** converted. You balances were updated accordingly.
* FAILED - an **error** has occurred with this conversion. Termporary statement.
* CANCELLED - the conversion didn't happen and has been cancelled.
  * Usually done by Flash Payments operations team.
