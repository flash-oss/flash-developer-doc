---
description: Generate Virtual Account Numbers for your clients for collection
---

# Sub-clients

The sub-client (aka _merchant_) feature allows you to create client accounts for deposit collection purposes. They can be issued for individuals as well as companies or other organisations.

{% hint style="info" %}
Note: This feature is **OFF** by default. Contact us if you want it.
{% endhint %}

Each sub-client will receive a dedicated **BSB** and **account number** that you or your clients can use to accept domestic AUD transfers within Australia. The **account name** is your sub-client's name. For companies - it's their `tradingAsName` or `legalName`. For individuals - it's their `fullName` (`firstName` + `middleName` + `lastName`).

{% hint style="warning" %}
Warning: The account number can only process local transfers, **no SWIFT/RTGS**.
{% endhint %}

All [deposits](deposits.md#querying-deposits) sent to your sub-client bank details are booked on your (master-client) account's balance. **Sub-clients can't have their own balances**.

Notifications via [webhooks](webhooks/webhooks.md) will provide important sub-client information as well.

### Creating a sub-client

{% hint style="warning" %}
This action creates real bank account. If you ever submit fake, unreal, testing, or incorrect data - you will be immediately **blocked** from FlashFX services.

Please add all possible precautions, processes, staff training, warning messages, and validation checks to you system(s) before creating a sub-client.

Please follow our latest requirements for the proper sub-client data submission:

1. Provide proper`firstName`and`lastName`
2. Provide proper`mobile`number
3. Provide proper `dob:` **the person must be under 65 years of age**&#x20;
4. Provide proper residential `address` including unit and street number. The sub-client address should correspond to your approved use case from the contract. By default you're only allowed to have local Australian sub-accounts. **Any non-Australian entities should be pre-approved based on their location and industry relevance for FlashFX.**  &#x20;
5. Provide proper`idDoc (type`, `docNumber` and `issuer)`based on the sub-client contact person address. For Australian residents either driver’s license or passport are accepted. For non-Australian residents only passport is accepted as a document type. Also, this should be a foreign passport ID, not local.
{% endhint %}

There are two types of sub-clients: `company` and `individual`.&#x20;

For every `company` registered as a sub-client there must be **one contact person individual data** submitted. Ideally the contact person should be a **company director** or have similar role. Therefore, if you are creating a sub-client of the `company` type, we require you to provide **extra** **details**:

* `legalName` - company legal name
* `businessNumber` - company business number (e.g ABN in Australia)

If the above fields are not set, the sub-client will be created as `individual` type.

{% hint style="info" %}
Note: The above personal data submission [requirements](sub-clients.md#creating-a-sub-client) should be as equally followed for the company contact person with an exception of `address` property, which can be a company address in this case. &#x20;
{% endhint %}

You can find the description of each field in the GraphQL API schema.

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createSubClient(
    input: {
      legalName: "Chineese Tradings"
      businessNumber: "330782000329701"
      firstName: "John"
      lastName: "Smith"
      email: "john.smith@example.com"
      mobile: "+61422832849"
      dob: "1979-05-12"
      address: {
        building: "25"
        street: "Xihu Road, Yuexiu District"
        suburb: "Guangzhou City"
        state: "Guangdong Province"
        postcode: "510030"
        country: CN
      }
      idDoc: {
        type: passport
        docNumber: "FF1948394"
        issuer: "AU"
      }
      externalId: "991188227733"
    }
  ) {
    success
    code
    message
    subClient {
      id
      legalName
      businessNumber
      fullName
      clientType
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
      bsb
      accountNo
      externalId
      # more properties available, see API schema
    }
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
        "clientType": "INDIVIDUAL",
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

You can also disable and activate sub-clients. Deposits sent to a disabled sub-client will no longer be booked against your balance.

#### Disabling a sub-client

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  disableSubClient(id: "606128f24bf29139b2cf74ef") {
    success
    code
    message
    subClient {
      id
      status
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "disableSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully disabled",
      "subClient": {
        "id": "606128f24bf29139b2cf74ef",
        "status": "DISABLED"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Activating a sub-client

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  activateSubClient(id: "606128f24bf29139b2cf74ef") {
    success
    code
    message
    subClient {
      id
      status
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "activateSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully activated",
      "subClient": {
        "id": "606128f24bf29139b2cf74ef",
        "status": "ACTIVE"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Available queries

#### Query for a single sub-client

{% tabs %}
{% tab title="Query" %}
```graphql
{
  subClient(id: "606d28675a2d931bc925fec2") {
    id
    fullName
    legalName
    tradingAsName
    clientType
    status
    primaryContact {
      firstName
      middleName
      lastName
      email
      dob
      mobile
    }
    address {
      building
      street
      suburb
      state
      country
      postcode
    }
    postalAddress {
      building
      street
      suburb
      state
      country
      postcode
    }
    businessNumber
    bsb
    accountNo
  	externalId
    fundingAccounts(input: { currencies: [EUR, USD, HKD, CNY] }) {
      iban
      accountNo
      bic
      currency
      externalReference
    }
  }
}

```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "subClient": {
      "id": "606d28675a2d931bc925fec2",
      "fullName": "ACME Corp",
      "legalName": "ACME Corp",
      "tradingAsName": null,
      "clientType": "COMPANY",
      "status": "ACTIVE",
      "primaryContact": {
        "firstName": "John",
        "middleName": null,
        "lastName": "Smith",
        "email": "john.smith@example.com",
        "dob": "1980-12-12",
        "mobile": "+61 422 832 849"
      },
      "address": {
        "building": "25",
        "street": "Moore St",
        "suburb": "Waterloo",
        "state": "NSW",
        "country": "AU",
        "postcode": "2017"
      },
      "postalAddress": {
        "building": "25",
        "street": "Moore St",
        "suburb": "Waterloo",
        "state": "NSW",
        "country": "AU",
        "postcode": "2017"
      },
      "businessNumber": "91383840265",
      "bsb": "802919",
      "accountNo": "1066419",
      "externalId": "991188227733"
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Query for multiple sub-clients

{% tabs %}
{% tab title="Query" %}
```graphql
{
  subClients {
    id
    fullName
    legalName
    clientType
    status
    businessNumber
    bsb
    accountNo
  	externalId
    # any other set of properties
  }
}

```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "subClients": [
      {
        "id": "5fb314cb9224595df522db61",
        "fullName": "Richard Smith",
        "legalName": null,
        "clientType": "INDIVIDUAL",
        "status": "ACTIVE",
        "businessNumber": null,
        "bsb": "802919",
        "accountNo": "1963041",
        "externalId": null
      },
      {
        "id": "60612f00a4d7dd5c96e37676",
        "fullName": "ABC Capital",
        "legalName": "ABC Co",
        "clientType": "COMPANY",
        "status": "ACTIVE",
        "businessNumber": "839399923932",
        "bsb": "802919",
        "accountNo": "1914920",
        "externalId": null
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Query for multiple sub-clients with filters

{% tabs %}
{% tab title="Query" %}
```graphql
{
  # more filters available, see SubClientQueryInput in API schema
  subClients(
    input: {
      clientType: INDIVIDUAL
      status: ACTIVE
      firstName: "John"
      lastName: "Smith"
      address: { country: AU }
    }
  ) {
    id
    fullName
    clientType
    status
    primaryContact {
      firstName
      lastName
    }
    address {
      country
    }
  }
}

```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "subClients": [
      {
        "id": "5fb314cb9224595df522db61",
        "fullName": "John Smith",
        "clientType": "INDIVIDUAL",
        "status": "ACTIVE",
        "primaryContact": {
          "firstName": "John",
          "lastName": "Smith"
        },
        "address": {
          "country": "AU"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

