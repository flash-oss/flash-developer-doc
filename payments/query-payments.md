# Query payments

#### Retrieving all your payments

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
    },
  },
  query: `
query ($input: PaymentQueryInput!) {
  payments(input: $input) {   
    id fromCurrency toCurrency 
  }
}`,
};  
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
 query($input: PaymentQueryInput!) {
  payments(input: $input) {
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
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "payments": [
      {
        "id": "5b04c62ec0bf606bf216ae21",
        "fromCurrency": "AUD",
        "toCurrency": "EUR"
      },
      {
        "id": "5b04c6bfc0bf606bf216af06",
        "fromCurrency": "AUD",
        "toCurrency": "USD"
      },
      {
        "id": "5b04c8e3c0bf606bf216b026",
        "fromCurrency": "EUR",
        "toCurrency": "AUD"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving some of your payments

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      statuses: "CLOSED",
      toCurrencies: ["EUR","USD"], 
    },
  },
  query: `
query ($input: PaymentQueryInput!) {
  payments(input: $input) {   
    id fromCurrency toCurrency 
  }
}`,
};    
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
  query($input: PaymentQueryInput!) {
  payments(input: $input) {
    id
    fromCurrency
    toCurrency
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
 {
  # there are more query parameters available, see the API schema
  "input": { 
    "statuses": "CLOSED",
    "toCurrencies": ["EUR","USD"]
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "payments": [
      {
        "id": "5b04c62ec0bf606bf216ae21",
        "fromCurrency": "AUD",
        "toCurrency": "EUR"
      },
      {
        "id": "5b04c6bfc0bf606bf216af06",
        "fromCurrency": "AUD",
        "toCurrency": "USD"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving a single payment

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables:{
    input: "5b04c62ec0bf606bf216ae21",
  },
  query: `
query ($input: ID) {  
  payment(id: $input) {
    status createdAt size
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ID) {
  payment(id: $input) {
    status
    createdAt
    size
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
{
  # there are more query parameters available, see the API schema
  "input": "5b04c62ec0bf606bf216ae21"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "payment": {
      "status": "CLOSED",
      "createdAt": "2018-08-13T05:45:28.698Z",
      "size": 1000
    }
  }
}
```
{% endtab %}
{% endtabs %}

