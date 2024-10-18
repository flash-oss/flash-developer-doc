---
description: You can query all your past conversions
---

# Query conversions

#### Retrieving all your conversions

{% tabs %}
{% tab title="Query" %}
```graphql
{
  conversions {
    id
    fromCurrency
    toCurrency
    rate
    # there are many other properties
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
{% tab title="Query" %}
```graphql
{
  # there are more query parameters available, see the API schema
  conversions(input: { statuses: CONVERTED, toCurrencies: [EUR USD] }) {
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
        "id": "6b04c62ec0bf606bf216ae21",
        "fromCurrency": "AUD",
        "toCurrency": "EUR"
      },
      {
        "id": "6b04c6bfc0bf606bf216af06",
        "fromCurrency": "AUD",
        "toCurrency": "USD"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Retrieving a single conversion

{% tabs %}
{% tab title="Query" %}
```graphql
{
  # there are more query parameters available, see the API schema
  conversion(id: "6b04c62ec0bf606bf216ae21") {
    status
    createdAt
    fromAmount
    toAmount
    # there are many other properties
  }
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

