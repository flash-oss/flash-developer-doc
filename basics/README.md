---
description: General information how to start using Flash FX API
---

# Basics

{% hint style="info" %}
### Assumptions

The examples below assume you are a verified customer of FlashFX and have been enabled for API access.
{% endhint %}

### GraphQL Playground

All the GraphQL queries can be sent via the [GraphQL Playground](https://api.flash-fx.com/) or as a HTTP POST request to `https://api.flash-fx.com`. Example:

```bash
curl -X POST 'https://api.flash-fx.com' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d '{
  "query":
    "{
       quote(input: {
         fromCurrency: AUD, toCurrency: USD, size: 9.9, currency: AUD
       })
       {
         bid ask symbol timestamp inverted
       }
     }"
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

In GraphQL Playground query editor press `Cmd+Space` or `Ctrl+Space` or `Opt+Space` or `Alt+Space` or `Shift+Space` to show context help and possible options.
{% endhint %}

Some of the GraphQL query parameters are required, others are optional. To understand if a variable/property is required you would need to check the API schema.

* Go to the [GraphQL Playground](https://api.flash-fx.com/) and click the button "**DOCS**" on the right.
* Browse through queries, mutations, input and output types. Find a variable/property which have an exclamation mark at the end. E.g. `fromCurrency: FromCurrency!`.
* The exclamation mark denotes that the variable/property is mandatory.

### Data cleansing is your responsibility

{% hint style="danger" %}
Do not ever send us `"N/A"` or `"NA"` or `"NULL"` or `"null"` or `"nil"` or any other dummy string values in any of the API fields.
{% endhint %}

### The user-agent HTTP header

It is a good idea to always send custom `user-agent` HTTP header value when doing requests to FlashFX API. Here is why:

* In case of troubleshooting we will be able to trace and mitigate your support questions **faster**.
* You might be **blocked** by our automated firewall if your user agent is something generic like `curl`, `Java-http-client`, `python-requests`, `Ruby`, etc.
