# Cleanse an address

### Address Cleanser

To cleanse an address, execute the `cleanseAddress` mutation as below. The result is returned synchronously — no polling or webhooks are required.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      street: "33 effingham st, sth launceston 7249",
      country: "AU",
    },
  },
  query: `
fragment AddressParts on Address { building streetNo street suburb state postcode country }

mutation ($input: AddressInput!) {
  cleanseAddress(input: $input) {
    success
    code
    message
    addressRequest {
      id
      originalAddress { ...AddressParts }
      cleansedAddress { ...AddressParts }
      recommendation
      score
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
fragment AddressParts on Address { building streetNo street suburb state postcode country }

mutation($input: AddressInput!) {
  cleanseAddress(input: $input) {
    success
    code
    message
    addressRequest {
      id
      originalAddress { ...AddressParts }
      cleansedAddress { ...AddressParts }
      recommendation
      score
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {
    "street": "33 effingham st, sth launceston 7249",
    "country": "AU"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "cleanseAddress": {
      "success": true,
      "code": "CLEANSED",
      "message": "Address cleansed",
      "addressRequest": {
        "id": "6820a4f3e1c2b5d8f0123456",
        "originalAddress": {
          "building": null,
          "streetNo": null,
          "street": "33 effingham st, sth launceston 7249",
          "suburb": null,
          "state": null,
          "postcode": null,
          "country": "AU"
        },
        "cleansedAddress": {
          "building": null,
          "streetNo": null,
          "street": "33 Effingham Street",
          "suburb": "South Launceston",
          "state": "TAS",
          "postcode": "7249",
          "country": "AU"
        },
        "recommendation": "approve",
        "score": 97.1
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
`originalAddress` and `cleansedAddress` are both of type `Address`, so the examples above declare a reusable [GraphQL fragment](https://graphql.org/learn/queries/#fragments) — `fragment AddressParts on Address { ... }` — and spread it into both with `...AddressParts` instead of repeating the field list.
{% endhint %}

**Address — `input`**

The standard `AddressInput` type is used:

| Field      | Description                                                               |
| ---------- | ------------------------------------------------------------------------- |
| `street`   | Street string, e.g. `Apt 256 123 John Ave`. **Required**                  |
| `country`  | Two-letter ISO 3166-1 alpha-2 country code, e.g. `AU`, `US`. **Required** |
| `building` | Building part of the address, where applicable (e.g. HK, SG). _Optional_  |
| `streetNo` | Street number, e.g. `33`. _Optional_                                      |
| `suburb`   | Suburb, city or other locality, e.g. `Paddington`. _Optional_             |
| `state`    | State of the country, e.g. `NSW`. _Optional_                              |
| `postcode` | Post code (aka zip code), e.g. `2000`. _Optional_                         |

Addresses from most countries worldwide are supported.

**Optional fields**

Only `street` and `country` are required. The other components — `building`, `streetNo`, `suburb`, `state`, `postcode` — are optional: you can either supply them separately, or fold them into `street` as a single free-form string (e.g. `"Unit 5, 33 Effingham St, South Launceston TAS 7249"`).

Either way, `originalAddress` keeps your input exactly as you sent it.

**Result — `recommendation` and `score`**

The result is an estimate, not a definitive check. Inspect `recommendation` (`approve`, `review`, `reject`) and `score` (0 to 100, higher is a closer match) on the returned `addressRequest` to decide how to treat the cleansed address. See the overview for details.

**Response codes**

| Code                            | Meaning                                                                        |
| ------------------------------- | ------------------------------------------------------------------------------ |
| `CLEANSED`                      | The address was cleansed and the result is in `addressRequest`                 |
| `ALREADY_CLEANSED`              | The same address was already cleansed today — the existing request is returned |
| `VALIDATION_ERROR`              | The input failed validation — e.g. `street` is missing                         |
| `CLEANSING_FAILED`              | The address could not be processed — the request is not billable               |
| `LIMIT_EXCEEDED`                | The free monthly request limit was reached                                     |
| `ADDRESS_CLEANSER_API_DISABLED` | The API is not enabled for your account — contact support                      |

**Deduplication**

If you submit the same address more than once within the same calendar day, the existing request is returned rather than creating a new one, with `code: ALREADY_CLEANSED`. You are billed at most once per day for the same address.

**Monthly request limit**

1,000 requests per month are included free of charge. Once reached, `success: false` is returned with `code: LIMIT_EXCEEDED`. Contact support to upgrade your account.
