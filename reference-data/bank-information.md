---
description: Ensuring bank details are correct
---

# Bank information

To validate BSB, BIC \(aka SWIFT code\) or IBAN use the bankInfo query.

{% hint style="info" %}
The `bankInfo` query accepts only one of `bsb`, `bic`, or `iban` arguments. Otherwise, it will return an error.
{% endhint %}

The below sample queries will return `null` if the BSB, BIC, IBAN is not found.

{% tabs %}
{% tab title="Query BSB" %}
```graphql
{
    bankInfo(input: { bsb: "012622" }) {
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

{% tab title="Query BIC" %}
```graphql
{
    bankInfo(input: { bic: "BARCGB22" }) {
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

{% tab title="Query IBAN" %}
```graphql
{
    bankInfo(input: { iban: "DE59500105178646768962" }) {
        name
        bic
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

{% tab title="Response IBAN" %}
```javascript
{
  "data": {
    "bankInfo": {
      "name": "BARCLAYS BANK PLC",
      "bic": "BARCGB22",
      "address": {
        "building": null,
        "street": "1 CHURCHILL PLACE",
        "suburb": "LONDON",
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

