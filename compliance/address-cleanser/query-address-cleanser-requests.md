# Query Address Cleanser requests

**Retrieving a single Address Cleanser request**

Use the `addressCleanserRequest` query with the `id` returned from the `cleanseAddress` mutation.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "6820a4f3e1c2b5d8f0123456",
  },
  query: `
query ($id: ID!) {
  addressCleanserRequest(id: $id) {
    id
    originalAddress {
      building
      streetNo
      street
      suburb
      state
      postcode
      country
    }
    cleansedAddress {
      building
      streetNo
      street
      suburb
      state
      postcode
      country
    }
    recommendation
    score
    createdAt
    billable
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($id: ID!) {
  addressCleanserRequest(id: $id) {
    id
    originalAddress {
      building
      streetNo
      street
      suburb
      state
      postcode
      country
    }
    cleansedAddress {
      building
      streetNo
      street
      suburb
      state
      postcode
      country
    }
    recommendation
    score
    createdAt
    billable
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "id": "6820a4f3e1c2b5d8f0123456"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "addressCleanserRequest": {
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
      "score": 97.1,
      "createdAt": "2026-06-12T08:23:14.521Z",
      "billable": true
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The query returns `null` if no request with the supplied `id` exists on your account.
{% endhint %}

**Retrieving all your Address Cleanser requests**

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {},
  },
  query: `
query ($input: AddressCleanserQueryInput) {
  addressCleanserRequests(input: $input) {
    id
    recommendation
    score
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: AddressCleanserQueryInput) {
  addressCleanserRequests(input: $input) {
    id
    recommendation
    score
    createdAt
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {}
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "addressCleanserRequests": [
      {
        "id": "6820a4f3e1c2b5d8f0123456",
        "recommendation": "approve",
        "score": 98.25,
        "createdAt": "2026-06-12T08:23:14.521Z"
      },
      {
        "id": "6820b1d9c3f4a7e2d0987654",
        "recommendation": "review",
        "score": 64.1,
        "createdAt": "2026-06-12T09:01:44.008Z"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

**Filtering Address Cleanser requests**

Use the `input` parameter to narrow results by recommendation or date range.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      recommendation: "approve",
      minCreatedAt: "2026-06-01T00:00:00.000Z",
      maxCreatedAt: "2026-07-01T00:00:00.000Z",
    },
  },
  query: `
query ($input: AddressCleanserQueryInput) {
  addressCleanserRequests(input: $input) {
    id
    recommendation
    score
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: AddressCleanserQueryInput) {
  addressCleanserRequests(input: $input) {
    id
    recommendation
    score
    createdAt
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {
    "recommendation": "approve",
    "minCreatedAt": "2026-06-01T00:00:00.000Z",
    "maxCreatedAt": "2026-07-01T00:00:00.000Z"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "addressCleanserRequests": [
      {
        "id": "6820a4f3e1c2b5d8f0123456",
        "recommendation": "approve",
        "score": 98.25,
        "createdAt": "2026-06-12T08:23:14.521Z"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

Available filter fields on `AddressCleanserQueryInput`:

| Field            | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| `recommendation` | Filter by recommendation — `approve`, `review`, or `reject`   |
| `minCreatedAt`   | Return only requests created after this timestamp (ISO 8601)  |
| `maxCreatedAt`   | Return only requests created before this timestamp (ISO 8601) |
