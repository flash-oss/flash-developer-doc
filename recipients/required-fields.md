---
description: >-
  Different countries support different account types, transfer mechanisms,
  payment delivery methods.
---

# Required fields

Understand what minimal data you need to have to successfully transfer a payment

#### List all the available payment delivery methods

{% tabs %}
{% tab title="Query" %}
```graphql
{
  availableDeliveryMethods {
    country
    currency
    method
    requiredFields
  }
}

```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "availableDeliveryMethods": [
      {
        "country": "AE",
        "currency": "AED",
        "method": "ACC_NO",
        "requiredFields": [
          "bic",
          "accountNo"
        ]
      },
      {
        "country": "AT",
        "currency": "EUR",
        "method": "IBAN",
        "requiredFields": [
          "bic",
          "iban"
        ]
      },
...
```
{% endtab %}
{% endtabs %}

The response above means that if you want to send euros to Austria \(AT\) then you'd need a BIC \(aka SWIFT\) code **and** an IBAN.

#### List some of the delivery methods

{% tabs %}
{% tab title="Query" %}
```graphql
{
  availableDeliveryMethods(input: { country: FR, currency: EUR }) {
    country
    currency
    method
    requiredFields
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "availableDeliveryMethods": [
      {
        "country": "FR",
        "currency": "EUR",
        "method": "ACC_NO",
        "requiredFields": [
          "bic",
          "accountNo"
        ]
      },
      {
        "country": "FR",
        "currency": "EUR",
        "method": "IBAN",
        "requiredFields": [
          "bic",
          "iban"
        ]
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

The response above means that to send euros to France \(FR\) you have to have ether of:

* BIC and an account number, or
* BIC and an IBAN.



