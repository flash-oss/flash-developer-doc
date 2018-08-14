---
description: Step by step instruction how to start using Flash FX API
---

# How to start

{% hint style="info" %}
You would need to be a verified customer of Flash FX. Also, we would need to enable API access for your account.
{% endhint %}

All the GraphQL queries can be sent via the [GraphQL Playground](https://api.flash-fx.com/) or as a HTTP POST request to `https://api.flash-fx.com`. Example:

```bash
curl -X POST 'https://api.flash-fx.com' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d '{
  "query":
    "{ quote(input: {fromCurrency: AUD, toCurrency: XRP, size: 9.9, currency: AUD}) { 
      bid ask symbol timestamp inverted
    } }"
}'
```

All the responses are JSON and have at least one property `data` and optional property `errors`.

```javascript
{
  "data": { ... },
  "errors": [ ... ]
}
```

{% hint style="info" %}
### Tip

In GraphQL Playground query editor press `Cmd+Space` or `Ctrl+Space` or `Opt+Space` or `Alt+Space` to show context help and possible options.
{% endhint %}

Some of the GraphQL query parameters are required, others are optional. To understand if a variable/property is required you would need to check the API schema.

* Go to the [GraphQL Playground](https://api.flash-fx.com/) and click the green button "SCHEMA" on the right.
* Browse through queries, mutations, input and output types. Find a variable/property which have an exclamation mark at the end. E.g. `fromCurrency: FromCurrency!`.
* The exclamation mark denotes that the variable/property is mandatory.

