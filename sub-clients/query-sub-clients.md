# Query sub-clients

### Available queries

#### Query for a single sub-client

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "606d28675a2d931bc925fec2",
      input: {
        currencies: ["EUR","USD","HKD","CNY"],
      },
  },
  query: `
query ($id: ID!, $input: FundingAccountQueryInput!) {
  subClient(id: $id) {
    id fullName legalName tradingAsNam clientType status 
    primaryContact {
      firstName middleName lastName email dob mobile
    }
    address {
      building street suburb state country postcode
    }
    postalAddress {
      building street suburb state country postcode
    } 
    businessNumber bsb accountNo externalId
    fundingAccounts(input: $input) {
      iban accountNo bic currency externalReference
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($id: ID!, $input: FundingAccountQueryInput!) {
  subClient(id: $id) {
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
    fundingAccounts(input: $input) {
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

{% tab title="Variables" %}
```javascript
{ 
  "id": "606d28675a2d931bc925fec2",
  "input": {
    "currencies": ["EUR", "USD", "HKD", "CNY"] 
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
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
    },
  },
  query: `
query ($input: SubClientQueryInput!) {
  subClients(input: $input) {   
    id fullName legalName clientType status businessNumber
    bsb accountNo externalId
  }
}`,
};  
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: SubClientQueryInput!){
  subClients(input: $input) {
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
