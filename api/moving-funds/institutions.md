---
description: Create, update and query the institutions that instruct your withdrawals
---

# Institutions

### What an Instructing Institution is <a href="#user-content-what-an-institution-is" id="user-content-what-an-institution-is"></a>

An instructing institution is another financial institution that instructed you to make a withdrawal. We record it for compliance reporting in Australia. It is not the sender of the money.

If nobody instructed you, do not send an institution. We then use your own company details for compliance and auditing.<br>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Institution explanation</p></figcaption></figure>

You use an institution in two places:

* `createWithdrawal`, in `instructingInstitutionId` or `instructingInstitution`. See [Withdraw funds](payouts/withdraw-funds.md).
* `createSubClient`, in the same two fields, when you create a virtual account on behalf of another financial institution. See [Create sub-clients](../accounts/virtual-account-numbers/create-sub-clients/).

### BIC and LEI <a href="#user-content-bic-and-lei" id="user-content-bic-and-lei"></a>

An institution can carry a `bic`, a  `lei`, or both. Please add at least one of them to every institution you use.&#x20;

The `institutions` query lists your institutions with their identifiers. Anyone can look up an LEI for free at [search.gleif.org](https://search.gleif.org/).

### Creating institutions

There are three ways to create an institution:

* in Flash Connect, on the Institutions page. Copy the institution `id` from there when you want to use it as `instructingInstitutionId`.
* via API with the `createInstitution` mutation.
* inside a withdrawal, in the `instructingInstitution` field of `createWithdrawal`. See [Withdraw funds.](payouts/withdraw-funds.md)

{% hint style="warning" %}
Create one institution per organisation. Institutions go into our compliance reports and our compliance team reviews each of them.
{% endhint %}

{% hint style="info" %}
Before creating or updating an institution we will try to find an existing one by:

* by `instructingInstitution.externalId`
* by `instructingInstitution.businessNumber` AND `instructingInstitution.address.country`
* by `instructingInstitution.legalName` AND `instructingInstitution.address.country`&#x20;

When we find one, `createInstitution` returns `INSTITUTION_ALREADY_EXISTS` together with that institution, so you can read its `id`. `bic` and `lei` are not used for matching.&#x20;
{% endhint %}

### Creating institution example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      legalName: "Intermediate Institution Ltd",
      bic: "BANKAU2S",
      lei: "5493001KJTIIGC8Y1R12",
      website: "example.com",
      businessNumber: "A39477669937",
      address: {
        street: "203 Business Street",
        suburb: "Sydney",
        state: "NSW",
        postcode: "2000",
        country: "AU",
      },
    },
  },
  query: `
mutation ($input: InstitutionInput!) {
  createInstitution(input: $input) {
    success code message
    institution {
      id legalName bic lei
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation ($input: InstitutionInput!) {
  createInstitution(input: $input) {
    success
    code
    message
    institution {
      id
      legalName
      bic
      lei
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {
    "legalName": "Intermediate Institution Ltd",
    "bic": "BANKAU2S",
    "lei": "5493001KJTIIGC8Y1R12",
    "website": "example.com",
    "businessNumber": "A39477669937",
    "address": {
      "street": "203 Business Street",
      "suburb": "Sydney",
      "state": "NSW",
      "postcode": "2000",
      "country": "AU"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "createInstitution": {
      "success": true,
      "code": "INSTITUTION_CREATED",
      "message": "Institution created",
      "institution": {
        "id": "65570da4f176682c5e412552",
        "legalName": "Intermediate Institution Ltd",
        "bic": "BANKAU2S",
        "lei": "5493001KJTIIGC8Y1R12"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### **Updating institutions**

Update an institution with the `updateInstitution` mutation and its `id`. The input always requires `legalName` and `address`, every other field is optional.

We apply only the non-empty fields you send. A field you leave out or send empty keeps its stored value, so you cannot blank a field this way. The `address` you send is merged over the stored one.

If nothing changes, we return `NO_CHANGES` and leave the institution untouched. Every real change goes to our compliance team for review again.

{% hint style="info" %}
Before we save, we check that no other institution of yours matches the new values by:

* `externalId`&#x20;
* `businessNumber` AND `address.country`
* `legalName` AND `address.country`

If a match is found, the update is rejected with the `INSTITUTION_ALREADY_EXISTS` code.
{% endhint %}

You can also send the new details inside `instructingInstitution` on your next withdrawal for that institution. When the details match an institution you already have, we use it for the withdrawal and apply your changes in the background. We do that at most once a day per institution.

### **Add an LEI to an existing institution**

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "65570da4f176682c5e412552",
    input: {
      legalName: "Intermediate Institution Ltd",
      lei: "5493001KJTIIGC8Y1R12",
      address: {
        country: "AU",
      },
    },
  },
  query: `
mutation ($id: ID, $input: InstitutionInput!) {
  updateInstitution(id: $id, input: $input) {
    success code message
    institution {
      id bic lei
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation ($id: ID, $input: InstitutionInput!) {
  updateInstitution(id: $id, input: $input) {
    success
    code
    message
    institution {
      id
      bic
      lei
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "id": "65570da4f176682c5e412552",
  "input": {
    "legalName": "Intermediate Institution Ltd",
    "lei": "5493001KJTIIGC8Y1R12",
    "address": {
      "country": "AU"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "updateInstitution": {
      "success": true,
      "code": "UPDATED",
      "message": "Institution 65570da4f176682c5e412552 updated.",
      "institution": {
        "id": "65570da4f176682c5e412552",
        "bic": null,
        "lei": "5493001KJTIIGC8Y1R12"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Querying institutions

The `institutions` query returns your institutions, newest first. Every field you set in `input` must match exactly. Leave `input` out to get all of them. The `institution` query returns one institution by its `id`.

#### List all institutions

The example below lists every institution with its identifiers.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {},
  },
  query: `
query ($input: InstitutionQueryInput) {
  institutions(input: $input) {
    id legalName externalId bic lei
    address {
      country
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query ($input: InstitutionQueryInput) {
  institutions(input: $input) {
    id
    legalName
    externalId
    bic
    lei
    address {
      country
    }
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
    "institutions": [
      {
        "id": "65570da4f176682c5e412552",
        "legalName": "Intermediate Institution Ltd",
        "externalId": "inst-001",
        "bic": null,
        "lei": "5493001KJTIIGC8Y1R12",
        "address": { "country": "AU" }
      },
      {
        "id": "6412a9c0e1f3b2a4c5d6e7f8",
        "legalName": "Other Remitter Pte Ltd",
        "externalId": "inst-002",
        "bic": null,
        "lei": null,
        "address": { "country": "SG" }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Find an institution by your own ID

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      externalId: "inst-002",
    },
  },
  query: `
query ($input: InstitutionQueryInput) {
  institutions(input: $input) {
    id legalName externalId bic lei
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query ($input: InstitutionQueryInput) {
  institutions(input: $input) {
    id
    legalName
    externalId
    bic
    lei
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "input": {
    "externalId": "inst-002"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "institutions": [
      {
        "id": "6412a9c0e1f3b2a4c5d6e7f8",
        "legalName": "Other Remitter Pte Ltd",
        "externalId": "inst-002",
        "bic": null,
        "lei": null
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}
