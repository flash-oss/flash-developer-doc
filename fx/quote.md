---
description: Quote the current bid and ask for a currency pair and size.
---

# Quote

## Reference/indicative quotes

Consider requesting an **indicative/reference** quote to understand the current currency trading rates before deciding to take action. Please be aware that market makers are not required to honor indicative quotes.

Paste this query to the API Playground to request an indicative quote.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      fromCurrency: "AUD",
      toCurrency: "USD",
      size: "10000",
      currency: "AUD",
    },
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

### Multiple reference/indicative quotes with a single HTTP request

If you send us too many API request - the system will automatically block you for a short period of time. To avoid that, we recommend sending multiple queries within a single HTTP request.

Typically, you want to collect several dozen FX prices with a sincle API call. The GraphQL allows sending multiple queries within one HTTP request. Below is an example of such query:

{% tabs %}
{% tab title="GraphQL Query" %}
```graphql
query {
  AUDUSD: quote(input: { fromCurrency: AUD, toCurrency: USD, size: 1000 }) { bid ask symbol }
  AUDEUR: quote(input: { fromCurrency: AUD, toCurrency: EUR, size: 1000 }) { bid ask symbol }
  AUDGBP: quote(input: { fromCurrency: AUD, toCurrency: GBP, size: 1000 }) { bid ask symbol }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "AUDUSD": {
      "bid": 0.76508,
      "ask": 0.76922,
      "symbol": "AUDUSD"
    },
    "AUDEUR": {
      "bid": 0.64588,
      "ask": 0.64942,
      "symbol": "AUDEUR"
    },
    "AUDGBP": {
      "bid": 0.57091,
      "ask": 0.57404,
      "symbol": "AUDGBP"
    }
  }
}
```
{% endtab %}
{% endtabs %}

Here is a JavaScript code to build and send the above query string:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const fromCurrencies = ["AUD"];
const toCurrencies = ["USD", "EUR", "GBP"];
const size = 1000;

let query = "";
for (const from of fromCurrencies) 
  for (const to of toCurrencies) 
    query += `${from}${to}: quote(input: { fromCurrency: ${from}, toCurrency: ${to}, size: ${size} }) { bid ask symbol }\n`;

query = "query {\n" + query + "\n}";

const response = await fetch("https://api.uat.flash-payments.com.au", { 
  method: "POST",
  headers: {
    authorization: "Bearer " + YOUR_TOKEN,
    "content-type": "application/json",
  },
  body: JSON.stringify({ query }),
});
const { data } = await response.json();
```
{% endtab %}
{% endtabs %}

Please be mindful. Do not fetch the rates you do not need. If there will be hundreds of `quote` queries within one HTTP request - it might timeout (take longer than 60 seconds).

## Tradable quote

In contrast to an indicative quote, a **tradable** quote is guaranteed by the market maker.

Paste this query to the GraphQL Playground to request a tradable quote. It is important to consider the `expireAt` date and time, and preserve the `id` of a tradable quote for future `createPayment` and `createConversion` calls.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      fromCurrency: "AUD",
      toCurrency: "USD",
      size: "10000",
      currency: "AUD",
      tradable: true,
    },
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
