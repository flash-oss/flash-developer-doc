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
{% tab title="Query" %}
```graphql
{
  # there are more query parameters available, see the API schema
  payments(input: { statuses: CLOSED, toCurrencies: [EUR USD] }) {
    id
    fromCurrency
    toCurrency
    # there are many other properties
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
{% tab title="Query" %}
```graphql
{
  # there are more query parameters available, see the API schema
  payment(id: "5b04c62ec0bf606bf216ae21") {
    status
    createdAt
    size
    # there are many other properties
  }
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

