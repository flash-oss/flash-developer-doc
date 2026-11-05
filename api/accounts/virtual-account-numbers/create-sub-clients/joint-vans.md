---
description: One virtual account for two to five people who hold it together
---

# Joint VANs

A joint sub-client is one virtual account that two to five people hold together, for example a married couple. We identify every person.

{% hint style="warning" %}
Joint sub-clients are currently available in the UAT environment. In production, `createSubClient` with `clientType: JOINT` returns `NOT_SUPPORTED` until we switch the feature on. Please follow the announcements in the [API change log](../../../basics/api-change-log.md).
{% endhint %}

### Creating a joint sub-client

Send `clientType: JOINT` and the details in `accountHolders`. Each entry describes one account holder. Follow these rules:

* Send two to five account holders. The order matters: the first holder becomes the `primaryContact` of the sub-client.
* Put the personal details in `accountHolders` only. Leave out the top-level `firstName`, `middleName`, `lastName`, `email`, `mobile`, `dob` and `idDoc`.
* Leave out `legalName`, `tradingAsName`, `businessNumber`, `orgType` and `principals`. A `JOINT` sub-client is for people, not for a business.
* Send one `address` for the sub-client, at the top level. We record it as one residential address for all holders.
* `externalId`, `instructingInstitutionId` and `postalAddress` work as for every other sub-client.

A `TRUST` or a `PARTNERSHIP` that two people run is a `COMPANY` sub-client with the matching [`orgType`](./#organisation-type), and the trustees or partners are its [principals](principals.md).

The [data quality requirements](./#data-quality-requirements) apply to every account holder: real people, real names, real documents. We check the request before we create anything. A request that breaks one of the rules above is rejected with an error, and no sub-client is created:

```json
{
  "errors": [
    {
      "message": "'accountHolders' needs two to five entries when 'clientType' is JOINT",
      "extensions": { "code": "BAD_USER_INPUT" }
    }
  ],
  "data": { "createSubClient": null }
}
```

An `externalId` that one of your sub-clients already has is rejected with `DUPLICATE_SUBCLIENT`, as for every sub-client.

For now, the **account name** of the joint virtual account is the first holder’s full name. For the example below it is `Duncan Brian Smith`. We will change it to list every holder, joined with “and” (`Duncan Brian Smith and Emily Smith`), and announce the change in the [API change log](../../../basics/api-change-log.md). A payer’s bank shows the account name when it checks the account before a payment.

You can read the holders back at any time via the `accountHolders` field of the `SubClient` type. See [Query sub-clients](../query-sub-clients.md#query-for-a-joint-sub-client). It is an empty list for `INDIVIDUAL` and `COMPANY` sub-clients.

{% hint style="info" %}
You cannot add or remove account holders, or change their details, after the sub-client is created. [`updateSubClient`](../disable-activate-and-update-sub-clients.md#updating-sub-clients) changes `externalId`, `address` and `postalAddress` only. If the holders change, create a new sub-client and disable the old one.
{% endhint %}

### Example

A joint VAN is registered to a married couple, Duncan and Emily Smith. Duncan’s ID is his passport. Emily’s ID is her driver licence.

{% hint style="info" %}
In the response, `primaryContact` and `fullName` come from the first holder.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      clientType: "JOINT",
      accountHolders: [
        {
          firstName: "Duncan",
          middleName: "Brian",
          lastName: "Smith",
          email: "duncan.smith@example.com",
          mobile: "+61422832841",
          dob: "1979-05-12",
          idDoc: {
            type: "passport",
            docNumber: "FF1948394",
            issuer: "Australian Passport Office (APO)",
            issueDate: "2020-12-12",
            expiryDate: "2030-12-12",
            country: "AU",
          },
        },
        {
          firstName: "Emily",
          lastName: "Smith",
          email: "emily.smith@example.com",
          mobile: "+61422832842",
          dob: "1981-03-04",
          idDoc: {
            type: "licence",
            docNumber: "12345678",
            issuer: "Service NSW",
            issueDate: "2022-06-01",
            expiryDate: "2027-06-01",
            country: "AU",
          },
        },
      ],
      address: {
        building: "25",
        street: "Moore St",
        suburb: "Waterloo",
        state: "NSW",
        postcode: "2017",
        country: "AU",
      },
      externalId: "991188227744",
    },
  },
  query: `
mutation ($input: CreateSubClientInput!) {
  createSubClient(input: $input) {
    success code message
    subClient {
      id fullName clientType status
      primaryContact {
        firstName middleName lastName email mobile dob
      }
      accountHolders {
        firstName middleName lastName email mobile dob
      }
      address {
        country
      }
      bsb accountNo externalId
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: CreateSubClientInput!) {
  createSubClient(input: $input) {
    success
    code
    message
    subClient {
      id
      fullName
      clientType
      status
      primaryContact {
        firstName
        middleName
        lastName
        email
        mobile
        dob
      }
      accountHolders {
        firstName
        middleName
        lastName
        email
        mobile
        dob
        # idDoc { type docNumber issuer issueDate expiryDate country } is available too
      }
      address {
        country
      }
      bsb
      accountNo
      externalId
      # more properties available, see API schema
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "clientType": "JOINT",
    "accountHolders": [
      {
        "firstName": "Duncan",
        "middleName": "Brian",
        "lastName": "Smith",
        "email": "duncan.smith@example.com",
        "mobile": "+61422832841",
        "dob": "1979-05-12",
        "idDoc": {
          "type": "passport",
          "docNumber": "FF1948394",
          "issuer": "Australian Passport Office (APO)",
          "issueDate": "2020-12-12",
          "expiryDate": "2030-12-12",
          "country": "AU"
        }
      },
      {
        "firstName": "Emily",
        "lastName": "Smith",
        "email": "emily.smith@example.com",
        "mobile": "+61422832842",
        "dob": "1981-03-04",
        "idDoc": {
          "type": "licence",
          "docNumber": "12345678",
          "issuer": "Service NSW",
          "issueDate": "2022-06-01",
          "expiryDate": "2027-06-01",
          "country": "AU"
        }
      }
    ],
    "address": {
      "building": "25",
      "street": "Moore St",
      "suburb": "Waterloo",
      "state": "NSW",
      "postcode": "2017",
      "country": "AU"
    },
    "externalId": "991188227744"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createSubClient": {
      "success": true,
      "code": "SUBCLIENT_CREATED",
      "message": "Sub-client was successfully created",
      "subClient": {
        "id": "690862b1c4f2a93d7e5f1a20",
        "fullName": "Duncan Brian Smith",
        "clientType": "JOINT",
        "status": "ACTIVE",
        "primaryContact": {
          "firstName": "Duncan",
          "middleName": "Brian",
          "lastName": "Smith",
          "email": "duncan.smith@example.com",
          "mobile": "+61 422 832 841",
          "dob": "1979-05-12"
        },
        "accountHolders": [
          {
            "firstName": "Duncan",
            "middleName": "Brian",
            "lastName": "Smith",
            "email": "duncan.smith@example.com",
            "mobile": "+61 422 832 841",
            "dob": "1979-05-12"
          },
          {
            "firstName": "Emily",
            "middleName": null,
            "lastName": "Smith",
            "email": "emily.smith@example.com",
            "mobile": "+61 422 832 842",
            "dob": "1981-03-04"
          }
        ],
        "address": {
          "country": "AU"
        },
        "bsb": "802919",
        "accountNo": "1066420",
        "externalId": "991188227744"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

In production, until the feature is switched on, the same request returns:

```json
{
  "data": {
    "createSubClient": {
      "success": false,
      "code": "NOT_SUPPORTED",
      "message": "Joint sub-clients are not available yet",
      "subClient": null
    }
  }
}
```
