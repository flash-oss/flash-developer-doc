---
description: Generate Virtual Account Numbers for your clients for collection
---

# Sub-clients

The sub-client feature allows you to create client accounts for deposit collection purposes. 

Each sub-client will receive a dedicated BSB and account number that you or your clients can use to accept domestic AUD transfers within Australia. 

All [deposits](deposits.md#querying-deposits) sent to your sub-client bank details are booked on your \(master-client\) account's balance and linked with the sub-client ID. Notifications via [webhook](webhooks/webhooks.md)s will provide important sub-client information as well.

{% hint style="info" %}
Note. This feature is **OFF** by default. Contact us if you want it.
{% endhint %}

### Creating a sub-client

There are two types of sub-clients: `company` and `individual`. 

If you are creating a sub-client of the company type, we require you to provide extra details:

* `legalName` - company legal name
* `businessNumber` - company business number \(e.g ABN in Australia\)

If the above fields are not set, the sub-client will be created as `individual` type.

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
      docType: passport
      docNumber: "FA1948394"
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



