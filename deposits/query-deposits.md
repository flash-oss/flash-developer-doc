# Query deposits

#### Query all deposits

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
    },
  },
  query: `
query ($input: DepositQueryInput!) {
  deposits(input: $input) {
    id amount currency status statusMessage externalId externalReference
    subClient {
      id fullName status clientType
    }    
    createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: DepositQueryInput!) {
  deposits(input: $input) {
    id
    amount
    currency
    status
    statusMessage
    externalId
    externalReference
    subClient {
      id
      fullName
      status
      clientType
    }
    createdAt
    # more fields available, see API schema
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
    "deposits": [
      {
        "id": "6053d4e0e3bc655e0598a742",
        "amount": 2000,
        "currency": "AUD",
        "status": "CONFIRMED",
        "statusMessage": "Transaction Confirmed",
        "externalId": "PR.1vcl",
        "externalReference": "FX1111",
        "subClient": null,
        "createdAt": "2021-03-18T22:32:01.010Z"
      },
      {
        "id": "6053d3319588389e1443587e",
        "amount": 40,
        "currency": "AUD",
        "status": "CONFIRMED",
        "statusMessage": "Transaction Confirmed",
        "externalId": "PR.1vci",
        "externalReference": "FX2121",
        "subClient": {
          "id": "5fb314cb9224595df522db61",
          "fullName": "John Doe",
          "status": "ACTIVE",
          "clientType": "INDIVIDUAL"
        },
        "createdAt": "2021-03-18T22:24:49.096Z"
      },
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Query deposit by ID

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables:{
    input: "6053d4e0e3bc655e0598a742",
  },
  query: `
query ($input: ID) {  
  deposit(id: $input) {
    id 
    amount currency status statusMessage externalId externalReference createdAt
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ID) {
  deposit(id: $input) {
    id
    amount
    currency
    status
    statusMessage
    externalId
    externalReference
    createdAt
    # more fields available, see API schema
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
{
  # there are more query parameters available, see the API schema
  "input": "6053d4e0e3bc655e0598a742"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "deposit": {
      "id": "6053d4e0e3bc655e0598a742",
      "amount": 2000,
      "currency": "AUD",
      "status": "CONFIRMED",
      "statusMessage": "Transaction Confirmed",
      "externalId": "PR.1vcl",
      "externalReference": "KX23249",
      "createdAt": "2021-03-18T22:32:01.010Z"
    }
  }
}
```
{% endtab %}
{% endtabs %}

