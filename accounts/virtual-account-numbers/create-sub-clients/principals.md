---
description: >-
  Who owns and controls a company sub-client: UBOs, directors, secretaries and
  trustees
---

# Principals

For every `company` sub-client we also want to know who owns and controls it. These people are the sub-client's **principals**: its ultimate beneficial owners, beneficial owners, directors, company secretaries and trustees. Our banking partners require this information for every company that holds a virtual account, and we pass it on to them when the sub-client's virtual accounts are issued.

Send them in the `principals` list of `createSubClient`. Each entry describes one person:

* `roles` - every role the person holds. Required, one or more of the values below
* `firstName` and `lastName` - required. Latin characters only, the same rules as for the contact person's name
* `middleName` - optional
* `dob` - date of birth in `YYYY-MM-DD` format, optional

One person can hold several roles:

* `ULTIMATE_BENEFICIAL_OWNER` - owns or controls at least 25% of the organisation, or over 10% where it is registered, licensed or regulated in a high-risk jurisdiction.
* `BENEFICIAL_OWNER` - owns a share below the `ULTIMATE_BENEFICIAL_OWNER` threshold.
* `DIRECTOR` - a member of the board or equivalent governing body.
* `SECRETARY` - the company secretary.
* `TRUSTEE` - holds the organisation's assets on trust for its beneficiaries.
* `OTHER` - significant control or influence that none of the other roles describes.

Principals are accepted for `company` sub-clients only: sending them for an `individual` is rejected with `INVALID_DATA`. Do not send an `id` when creating a sub-client - we assign one to every principal and return it in the response, so that you can [edit that person later](../disable-activate-and-update-sub-clients.md#updating-sub-client-principals). [The data quality requirements](./#data-quality-requirements) apply to principals as well: real people, real names, real dates of birth.

{% hint style="info" %}
Principals are optional for now. They will become mandatory for every `company` sub-client - we will announce the date in advance in the [API change log](../../../basics/api-change-log.md). Please start sending them with every new company sub-client and add them to your existing ones via `updateSubClient`. The same details can be recorded in Flash Connect.&#x20;
{% endhint %}

You can read them back at any time via the `principals` field of the `SubClient` type (`id`, `roles`, `firstName`, `middleName`, `lastName`, `dob`). It is an empty list for `individual` sub-clients.

#### Creating a sub-client with principals

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: { 
    input: {
      legalName: "Chineese Tradings", 
      businessNumber: "330782000329701",
      orgType: "COMPANY", 
      firstName: "John", 
      lastName: "Smith", 
      email: "john.smith@example.com",
      mobile: "+61422832849",
      dob: "1979-05-12",
      address: {
        building: "25",
        street: "Xihu Road, Yuexiu District",
        suburb: "Guangzhou City",
        state: "Guangdong Province",
        postcode: "510030",
        country: "CN",
      },
      idDoc: {
        type: "passport",
        docNumber: "FF1948394",
        issuer: "Australian Passport Office (APO)",
        issueDate: "2000-01-01",
        expiryDate: "2045-01-01",
        country: "AU",
      },
      principals: [
        {
          firstName: "Wei",
          lastName: "Zhang",
          dob: "1975-03-02",
          roles: ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"],
        },
        {
          firstName: "Mei",
          middleName: "Ling",
          lastName: "Lin",
          roles: ["BENEFICIAL_OWNER"],
        },
      ],
      externalId: "991188227733",
    },
  },
  query: `
mutation ($input: CreateSubClientInput!) {
  createSubClient(input: $input) {
    success code message
    subClient {
      id legalName businessNumber fullName clientType orgType status
      primaryContact {
        firstName lastName email mobile dob
      }
      address {
        country
      }
      principals {
        id roles firstName middleName lastName dob
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
      legalName
      businessNumber
      fullName
      clientType
      orgType
      status
      primaryContact {
        firstName
        lastName
        email
        mobile
        dob
      }
      address {
        country
      }
      principals {
        id
        roles
        firstName
        middleName
        lastName
        dob
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
    "legalName": "Chineese Tradings",
    "businessNumber": "330782000329701",
    "orgType": "COMPANY",
    "firstName": "John",
    "lastName": "Smith",
    "email": "john.smith@example.com",
    "mobile": "+61422832849",
    "dob": "1979-05-12",
    "address": {
      "building": "25",
      "street": "Xihu Road, Yuexiu District",
      "suburb": "Guangzhou City",
      "state": "Guangdong Province",
      "postcode": "510030",
      "country": "CN"
    },
    "idDoc": {
      "type": "passport",
      "docNumber": "FF1948394",
      "issuer": "Australian Passport Office (APO)",
      "issueDate": "2000-01-01",
      "expiryDate": "2045-01-01",
      "country": "AU"     
    },
    "principals": [
      {
        "firstName": "Wei",
        "lastName": "Zhang",
        "dob": "1975-03-02",
        "roles": ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"]
      },
      {
        "firstName": "Mei",
        "middleName": "Ling",
        "lastName": "Lin",
        "roles": ["BENEFICIAL_OWNER"]
      }
    ],
    "externalId": "991188227733"
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
        "id": "606d28675a2d931bc925fec2",
        "legalName": "Chineese Tradings",
        "businessNumber": "330782000329701",
        "fullName": "John Smith",
        "clientType": "C",
        "orgType": "COMPANY",
        "status": "ACTIVE",
        "primaryContact": {
          "firstName": "John",
          "lastName": "Smith",
          "email": "john.smith@example.com",
          "mobile": "+61 422 832 849",
          "dob": "1979-05-12"
        },
        "address": {
          "country": "CN"
        },
        "principals": [
          {
            "id": "68d4c3a1f2b9e07c4d1a5b01",
            "roles": ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"],
            "firstName": "Wei",
            "middleName": null,
            "lastName": "Zhang",
            "dob": "1975-03-02"
          },
          {
            "id": "68d4c3a1f2b9e07c4d1a5b02",
            "roles": ["BENEFICIAL_OWNER"],
            "firstName": "Mei",
            "middleName": "Ling",
            "lastName": "Lin",
            "dob": null
          }
        ],
        "bsb": "802919",
        "accountNo": "1066419",
        "externalId": "991188227733"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}



