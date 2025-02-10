---
description: General information how to start using Flash Payments API
---

# Basics

{% hint style="info" %}
### Assumptions

The examples below assume you are a verified customer of Flash Payments and have been enabled for API access.
{% endhint %}

### GraphQL Playground

All the GraphQL queries can be sent via the [GraphQL Playground](https://api.uat.flash-payments.com.au/) or as a HTTP POST request to `https://api.uat.flash-payments.com.au`. Example:

```bash
echo '{
  "query":
    "{
       quote(input: {
         fromCurrency: AUD, toCurrency: USD, size: 9.9, currency: AUD
       })
       {
         bid ask symbol timestamp inverted
       }
     }"
}' | curl -X POST 'https://api.uat.flash-payments.com.au' \
-H 'authorization: Bearer YOUR_TOKEN' \
-H 'content-type: application/json' \
-d @-
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

* Go to the [GraphQL Playground](https://api.uat.flash-payments.com.au/) and click the button "**DOCS**" on the right.
* Browse through queries, mutations, input and output types. Find a variable/property which have an exclamation mark at the end. E.g. `fromCurrency: FromCurrency!`.
* The exclamation mark denotes that the variable/property is mandatory.

### Data cleansing is your responsibility

{% hint style="danger" %}
Do not ever send us `"N/A"` or `"NA"` or `"NULL"` or `"null"` or `"nil"` or any other dummy string values in any of the API fields.
{% endhint %}

### The user-agent HTTP header

It is a good idea to always send custom `user-agent` HTTP header value when doing requests to Flash Payments API. Here is why:

* In case of troubleshooting we will be able to trace and mitigate your support questions **faster**.
* You might be **blocked** by our automated firewall if your user agent is something generic like `curl`, `Java-http-client`, `python-requests`, `Ruby`, etc.

### 429 Too many requests

Our API have smart monitoring. It might temporary block your IP address if it thinks you are abusing the system. There are many various scenarios when you can be blocked, but we won't going to disclose them at any point of time due to security reasons.
