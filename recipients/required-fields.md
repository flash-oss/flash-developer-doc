---
description: >-
  Different countries support different account types, transfer mechanisms,
  payment delivery methods.
---

# Delivery methods

Please verify that you have all the required data to initiate a successful payment transfer.

#### List all the available payment delivery methods

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables:{
    input:{
    },
  }, 
  query: `
query ($input: AvailableDeliveryMethodsInput!) {
  availableDeliveryMethods(input: $input) {
    country currency method requiredFields
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: AvailableDeliveryMethodsInput!) {
  availableDeliveryMethods(input: $input) {
    country
    currency
    method
    requiredFields
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

The response above means that if you want to send euros to Austria (AT) then you'd need a BIC (aka SWIFT) code **and** an IBAN.

#### List some of the delivery methods

{% tabs %}
{% tab title="JavaScript" %}
```graphql
const bodyJSON = {
  variables:{
    input:{
      country: "FR",
      currency: "EUR",
    },
  }, 
  query: `
query ($input: AvailableDeliveryMethodsInput!) {
  availableDeliveryMethods(input: $input) {
    country currency method requiredFields
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: AvailableDeliveryMethodsInput!) {
  availableDeliveryMethods(input: $input) {
    country
    currency
    method
    requiredFields
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```
{
  "input": {
    "country": "FR", 
    "currency": "EUR" 
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

The response above means that to send euros to France (FR) you have to have ether of:

* BIC and an account number, or
* BIC and an IBAN.
