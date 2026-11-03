# Query sub-clients

### Available queries

#### Query for a single sub-client

`aliases` lists the sub-client's [PayIDs](sub-client-payids.md). `principals` lists the people who own or control a `company` sub-client. `accountHolders` lists the people who hold a [joint VAN](create-sub-clients/joint-vans.md). Each of these fields is resolved separately for every sub-client in a response, so request them only where you need them. `accountHolders` is an empty list unless `clientType` is `JOINT`.

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
    id fullName legalName tradingAsName clientType status 
    primaryContact {
      firstName middleName lastName email dob mobile
    }
    address {
      building street suburb state country postcode
    }
    postalAddress {
      building street suburb state country postcode
    } 
    principals {
      id roles firstName middleName lastName dob
    }
    businessNumber bsb accountNo externalId
    aliases {
      alias aliasType status createdAt
    }
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
    principals {
      id
      roles
      firstName
      middleName
      lastName
      dob
    }
    businessNumber
    bsb
    accountNo
    externalId
    aliases {
      alias
      aliasType
      status
      createdAt
    }
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
      "principals": [
        {
          "id": "68d4c3a1f2b9e07c4d1a5b11",
          "roles": ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"],
          "firstName": "John",
          "middleName": null,
          "lastName": "Smith",
          "dob": "1980-12-12"
        },
        {
          "id": "68d4c3a1f2b9e07c4d1a5b12",
          "roles": ["BENEFICIAL_OWNER"],
          "firstName": "Jane",
          "middleName": null,
          "lastName": "Smith",
          "dob": null
        }
      ],
      "businessNumber": "91383840265",
      "bsb": "802919",
      "accountNo": "1066419",
      "externalId": "991188227733",
      "aliases": [
        {
          "alias": "acme-corp@example.com",
          "aliasType": "EMAIL",
          "status": "ACTIVE",
          "createdAt": "2026-10-08T01:12:33.481Z"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Query for a joint sub-client

`accountHolders` returns every holder with their contact details and `idDoc`. The first holder is also the `primaryContact`.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "690862b1c4f2a93d7e5f1a20",
  },
  query: `
query ($id: ID!) {
  subClient(id: $id) {
    id fullName clientType status
    primaryContact {
      firstName middleName lastName email dob mobile
    }
    accountHolders {
      firstName middleName lastName email mobile dob
      idDoc {
        type docNumber issuer issueDate expiryDate country
      }
    }
    address {
      building street suburb state country postcode
    }
    bsb accountNo externalId
  }
}`,
}; 
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($id: ID!) {
  subClient(id: $id) {
    id
    fullName
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
    accountHolders {
      firstName
      middleName
      lastName
      email
      mobile
      dob
      idDoc {
        type
        docNumber
        issuer
        issueDate
        expiryDate
        country
      }
    }
    address {
      building
      street
      suburb
      state
      country
      postcode
    }
    bsb
    accountNo
    externalId
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "id": "690862b1c4f2a93d7e5f1a20"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
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
        "dob": "1979-05-12",
        "mobile": "+61 422 832 841"
      },
      "accountHolders": [
        {
          "firstName": "Duncan",
          "middleName": "Brian",
          "lastName": "Smith",
          "email": "duncan.smith@example.com",
          "mobile": "+61 422 832 841",
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
          "middleName": null,
          "lastName": "Smith",
          "email": "emily.smith@example.com",
          "mobile": "+61 422 832 842",
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
        "country": "AU",
        "postcode": "2017"
      },
      "bsb": "802919",
      "accountNo": "1066420",
      "externalId": "991188227744"
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

To list the sub-clients of one type, filter by `clientType`: `INDIVIDUAL`, `COMPANY` or `JOINT`.

{% tabs %}
{% tab title="JavaSscript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      clientType: "INDIVIDUAL",
      status: "ACTIVE",
      firstName: "John",
      lastName: "Smith",
      address: { 
        country: "AU", 
      },
    },
  },
  query: `
query ($input: SubClientQueryInput!) {
  subClients(input: $input) {   
    id fullName clientType status 
    primaryContact {
      firstName lastName
    }
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
query($input: SubClientQueryInput!){
  subClients(input: $input) {
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
    # any other set of properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
<pre class="language-graphql"><code class="lang-graphql">{   
# more filters available, see SubClientQueryInput in API schema
<strong>  "input": {
</strong>    "clientType": "INDIVIDUAL",
    "status": "ACTIVE",
    "firstName": "John",
    "lastName": "Smith",
    "address": { 
      "country": "AU" 
    }
  }
}
</code></pre>
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
