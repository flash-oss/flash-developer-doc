---
description: Quote the current bid and ask for a currency pair and size.
---

# Quote

Consider requesting an **indicative** quote to understand the current currency trading rates before deciding to take action. Please be aware that market makers are not required to honor indicative quotes.

Paste this query to the API Playground to request an indicative quote.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables:{
    input:{
      "fromCurrency":"AUD",
      "toCurrency":"USD",
      "size":"10000",
      "currency":"AUD"
    }
  },
  query: `
query ($input: QuoteInput!) { 
  quote(input: $input) {   
    bid ask symbol timestamp inverted 
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
<pre class="language-graphql"><code class="lang-graphql"><strong>query($input: QuoteInput!) {
</strong>  quote(input: $input) {
    bid
    ask
    symbol
    timestamp
    inverted
  }
}
</code></pre>
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "fromCurrency": "AUD", 
    "toCurrency": "USD", 
    "size": "10000", 
    "currency": "AUD"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "quote": {
      "bid": 0.61104,
      "ask": 0.62077,
      "symbol": "USDAUD",
      "timestamp": "2018-08-13T07:54:54.993Z",
      "inverted": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

In contrast to an indicative quote, a **tradable** quote is guaranteed by the market maker.

Paste this query to the GraphQL Playground to request a tradable quote. It is important to consider the `expireAt` date and time, and preserve the `id` of a tradable quote for future `createPayment` and `createConversion` calls.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables:{
    input:{
      "fromCurrency":"AUD",
      "toCurrency":"USD",
      "size":"10000",
      "currency":"AUD",
      "tradable": true
    }
  },
  query: `
query ($input: QuoteInput!) { 
  quote(input: $input) {   
    bid ask symbol timestamp inverted expireAt id
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: QuoteInput!) {
  quote(input: $input) {
    bid
    ask
    symbol
    timestamp
    inverted
    expireAt
    id
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
  "input": { 
    "fromCurrency": "AUD", 
    "toCurrency": "USD", 
    "size": "10000", 
    "currency": "AUD",
    "tradeable": true 
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "quote": {
      "bid": 0.61339,
      "ask": 0.63101,
      "symbol": "AUDUSD",
      "timestamp": "2025-03-04T01:53:08.829Z",
      "inverted": false,
      "expireAt": "2025-03-04T01:55:08.830Z",
      "id": "67c65d04e5086d18751617ba"
    }
  }
}
```
{% endtab %}
{% endtabs %}
