---
description: CRUD queries for your payment senders
---

# Senders

We are legally obliged to collect the actual sender details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

If you want to receive funds from yourself then please provide your own details. See the DOCS in [Playground](https://api.uat.flash-payments.com.au/) for other sender details options.

* `sender` and `senders` queries - **read** your address book.
* `createSender` - **creates** a new record in the Flash Payments database.
* `updateSender` - **updates** an existing sender.
* `deleteSender` - **deletes** an existing sender.

#### Query single sender

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "59f2733f2519e236edab0efe",
  },
  query: `
query ($input: ID) {
  sender(id: $input) {
    email firstName lastName companyName
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
query($input: ID) {
  sender(id: $input) {
    email
    firstName
    lastName
    companyName
    address {
      country
    }
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": "59f2733f2519e236edab0efe"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "sender": {
      "email": "john@example.com",
      "firstName": "John",
      "lastName": "Smith",
      "companyName": null,
      "address": {
        "country": "GB"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Query multiple senders

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      email: "john@example.com",
    },
  },
  query: `
query ($input: RecipientQueryInput!) {
  senders(input: $input) {
    email firstName lastName companyName
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
query($input: SenderQueryInput!) {
  senders(input: $input) {
    email
    firstName
    lastName
    companyName
    address {
      country
    }
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "email": "john@example.com" 
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "senders": [
      {
        "email": "john@example.com",
        "firstName": "John",
        "lastName": "Smith",
        "companyName": null,
        "address": {
          "country": "GB"
        }
      },
      {
        "email": "john@example.com",
        "firstName": null,
        "lastName": null,
        "companyName": "Acme Inc",
        "address": {
          "country": "US"
        }
      },
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Create a sender

{% hint style="info" %}
In addresses the`suburb`field is an Australian suburb. For other countries you should put the city (e.g. Manila or London) or any other small administrative area name.

If you find it technically challenging to submit all components of the sender’s address, we would appreciate it if you could at least collect the sender’s country along with a complete address string that includes the postcode and put them into the `country` and `street` fields, respectively. In this case, you can skip the `postcode`, `suburb`, and `state` fields, and the sender record will still be created.
{% endhint %}

{% hint style="info" %}
The date of birth (`dob`) is not mandatory. However, if it is not provided, your transactions may undergo additional compliance reviews, which can lead to longer processing times—potentially several hours or days instead of seconds. Please also be aware that this may result in additional fees to cover the extra effort involved.
{% endhint %}

#### Create an Individual sender

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      firstName:"Malcolm",
      lastName:"Jez",
      dob:"2000-01-01",
      email:"malcolm@example.com",
      mobile:"+61 4123456789",
      address: {
        street:"1 Test St",
        suburb:"London",
        state:"TST",
        country:"GB",
        postcode:"2000",
      },
      idDoc: {
        type:"passport",
        docNumber:"GB1234321",
        issuer:"His Majesty’s Passport Office (HMPO)",
        issueDate:"1990-01-01",
        expiryDate:"2045-01-01",
        country:"GB",
      },
    },
  },
  query: `
mutation ($input: SenderInput!) {
  createSender(input: $input) {
    success code message
    sender {
      id nickName
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: SenderInput!) {
  createSender(input: $input) {
    success code message
    sender {
      id 
      nickName
      # there are many other properties
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "firstName": "Malcolm",
    "lastName": "Jez",
    "dob": "2000-01-01",
    "email": "malcolm@example.com",
    "mobile": "+61 4123456789", 
    "address": {
      "street": "1 Test St", 
      "suburb": "London", 
      "state": "TST", 
      "country": "GB", 
      "postcode": "2000"
    },
    "idDoc": {
      "type": "passport", 
      "docNumber": "GB1234321", 
      "issuer": "His Majesty’s Passport Office (HMPO)", 
      "issueDate": "1990-01-01", 
      "expiryDate": "2045-01-01", 
      "country": "GB"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createSender": {
      "success": true,
      "code": "CREATED",
      "message": "New sender created",
      "sender": {
        "id": "686393e689c1fb1b255cac5c",
        "nickName": "MalcolmJez"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Create a Company  sender

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      companyName: "Acme Pte Ltd",
      businessNumber: "12345678912",
      email: "acme@example.com",
      mobile: "+61 4123456789",
      address: {
        street: "1 Test St",
        suburb: "London",
        state: "TST",
        country: "GB",
        postcode: "2000",
      },
      idDoc: {
        type: "certificateOfRegistration",
        docNumber: "GB-REG-987654321",
        issuer: "Companies House",
        issueDate: "1990-01-01",
        expiryDate: "2100-01-01",
        country: "GB",
      },
    },
  },
  query: `
mutation ($input: SenderInput!) {
  createSender(input: $input) {
    success code message 
    sender { 
      id nickName    
    }  
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: SenderInput!) {
  createSender(input: $input) {
    success code message
    sender {
      id 
      nickName
      # there are many other properties
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "companyName": "Acme Pte Ltd", 
    "businessNumber": "12345678912", 
    "email": "acme@example.com", 
    "mobile": "+61 4123456789", 
    "address": {
      "street": "1 Test St", 
      "suburb": "London", 
      "state": "TST", 
      "country": "GB", 
      "postcode": "2000"
    },
    "idDoc": {
      "type": "certificateOfRegistration", 
      "docNumber": "GB-REG-987654321", 
      "issuer": "Companies House", 
      "issueDate": "1990-01-01", 
      "expiryDate": "2100-01-01", 
      "country": "GB"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```
{
  "data": {
    "createSender": {
      "success": true,
      "code": "CREATED",
      "message": "New sender created",
      "sender": {
        "id": "68638e6989c1fb1b255ca9c3",
        "nickName": "Acme Pte L"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Update sender

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  updateSender(
    id: "5ca18312ace1db0af5784826"
    input: {
      firstName: "Malcolm"
      lastName: "Jez The Seconds"
      dob: "2000-01-01"
      email: "malcolm@example.com"
      mobile: "+61 4123456789"
      address: {
        street: "1 Test St"
        suburb: "London"
        state: "TST"
        country: GB
        postcode: "2001"
      }
      idDoc: {
        type: passport
        docNumber: "GB1234321"
        issuer: "His Majesty’s Passport Office (HMPO)"
        issueDate: "1990-01-01"
        expiryDate: "2045-01-01"
        country: GB
      }
    }
  ) {
    success
    code
    message
    sender {
      id
      # there are many other properties
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "updateSender": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sender updated",
      "sender": {
        "id": "5ca18312ace1db0af5784826"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Delete sender

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  deleteSender(id: "5ca18d25ace1db0af5784893") {
    success code message
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "deleteSender": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sender deleted"
    }
  }
}
```
{% endtab %}
{% endtabs %}
