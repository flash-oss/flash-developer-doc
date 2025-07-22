---
description: You can query all your past conversions
---

# Query conversions

#### Retrieving all your conversions

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
    },
  },
  query: `
query ($input: ConversionQueryInput!) {
  conversions(input: $input) {   
    id fromCurrency toCurrency rate 
  }
}`,
};    
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
 query($input: ConversionQueryInput!) {
  conversions(input: $input) {
    id
    fromCurrency
    toCurrency
    rate
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
  "input": { 
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "conversions": [
      {
        "id": "6b04c62ec0bf606bf216ae21",
        "fromCurrency": "AUD",
        "toCurrency": "EUR",
        "rate": 0.71
      },
      {
        "id": "6b04c6bfc0bf606bf216af06",
        "fromCurrency": "AUD",
        "toCurrency": "USD",
        "rate": 0.70
      },
      {
        "id": "6b04c8e3c0bf606bf216b026",
        "fromCurrency": "EUR",
        "toCurrency": "AUD",
        "rate": 0.71
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving some of your conversions

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      statuses: "CONVERTED",
      toCurrencies: ["EUR","USD"], 
    },
  },
  query: `
query ($input: ConversionQueryInput!) {
  conversions(input: $input) {   
    id fromCurrency toCurrency rate 
  }
}`,
};    
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ConversionQueryInput!) {
  conversions(input: $input) {
    id
    fromCurrency
    toCurrency
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": { 
    "statuses": "CONVERTED",
    "toCurrencies": ["EUR","USD"]
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "conversions": [
      {
        "id": "6833ad9b4e94acce5d4a031b",
        "fromCurrency": "AUD",
        "toCurrency": "USD"
      },
      {
        "id": "678a18bb2879c374b378ee71",
        "fromCurrency": "AUD",
        "toCurrency": "EUR"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving a single conversion by ID

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "6b04c62ec0bf606bf216ae21",
  },
  query: `
query ($input: ID) {  
  conversion(id: $input) {
    status createdAt fromAmount toAmount 
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ID) {
  conversion(id: $input) {
    status
    createdAt
    fromAmount
    toAmount
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": "6b04c62ec0bf606bf216ae21"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "conversion": {
      "status": "CONVERTED",
      "createdAt": "2024-08-13T05:45:28.698Z",
      "fromAmount": 1000,
      "toAmount": 710.01
    }
  }
}
```
{% endtab %}
{% endtabs %}

