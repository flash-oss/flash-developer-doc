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

The status of a conversion can be any of the following:

* INITIALISED — A conversion record was created, but no further action has been taken.\
  This status typically does not occur via the API.
* PENDING — The conversion is in progress. This is the usual status immediately after creation.
* CONVERTED — The conversion completed successfully. Your balances have been updated accordingly.
* FAILED — An error occurred during the conversion. This is usually a temporary state.
* CANCELLED — The conversion was not completed and has been cancelled.\
  This action is typically performed by the Flash Payments operations team.
