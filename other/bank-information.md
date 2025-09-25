---
description: Ensuring bank details are correct
---

# Bank information

To validate BSB, BIC (aka SWIFT code) or IBAN use the bankInfo query.

{% hint style="info" %}
The `bankInfo` query accepts only one of `bsb`, `bic`, or `iban` arguments. Otherwise, it will return an error.
{% endhint %}

The below sample queries will return `null` if the BSB, BIC, IBAN is not found.

Validate BSB

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      bsb: "012622",
    },
  }, 
  query: `
query ($input: BankInfoQueryInput!) {
  bankInfo(input: $input) {
    name 
    address {
      building street suburb state country postcode
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: BankInfoQueryInput!){
  bankInfo(input: $input) {
    name
    address {
      building
      street
      suburb
      state
      country
      postcode
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
  "input": {
    "bsb": "012622"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "bankInfo": {
      "name": "ANZ",
      "address": {
        "building": null,
        "street": "Shop 1  Westfield Shopping Ctr",
        "suburb": "Figtree",
        "state": "NSW",
        "country": "AU",
        "postcode": "2525"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Validate BIC

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      bic: "BARCGB22",
    },
  }, 
  query: `
query ($input: BankInfoQueryInput!) {
  bankInfo(input: $input) {
    name 
    address {
      building street suburb state country postcode
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: BankInfoQueryInput!){
  bankInfo(input: $input) {
    name
    address {
      building
      street
      suburb
      state
      country
      postcode
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
  "input": {
    "bic": "BARCGB22"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "bankInfo": {
      "name": "BARCLAYS BANK PLC",
      "address": {
        "building": null,
        "street": "1 CHURCHILL PLACE, CANARY WHARF London",
        "suburb": "London",
        "state": null,
        "country": "GB",
        "postcode": "E14 5HP"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Validate IBAN

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      iban: "DE59500105178646768962",
    },
  }, 
  query: `
query ($input: BankInfoQueryInput!) {
  bankInfo(input: $input) {
    name 
    address {
      building street suburb state country postcode
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: BankInfoQueryInput!){
  bankInfo(input: $input) {
    name
    address {
      building
      street
      suburb
      state
      country
      postcode
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
  "input": {
    "iban": "DE59500105178646768962"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "bankInfo": {
      "name": "ING-DIBA AG (RETAIL BANKING)",
      "address": {
        "building": null,
        "street": "THEODOR-HEUSS-ALLEE 2 Frankfurt Am Main",
        "suburb": "Frankfurt Am Main",
        "state": null,
        "country": "DE",
        "postcode": "60486"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

