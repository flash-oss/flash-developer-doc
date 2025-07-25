---
description: CRUD queries for your payment recipients
---

# Recipients

We are legally obliged to collect the actual recipient details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

Please, send us the final funds recipient. If sending to self then please provide your own details. See the DOCS in [Playground](https://api.uat.flash-payments.com.au/) for other recipient details options.

* `recipient` and `recipients` queries - **read** your address book.
* `createRecipient` - **creates** a new record in the Flash Payments database.
* `updateRecipient` - **updates** an existing recipient.
* `deleteRecipient` - **deletes** an existing recipient.

#### Query a single recipient

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "6b04c62ec0bf606bf216ae21",
  },
  query: `
query ($input: ID) {
  recipient(id: $input) {
    accountIdType currency country email
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: ID) {
  recipient(id: $input) {
    accountIdType
    currency
    country
    email
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```
{
  "input": "6b04c62ec0bf606bf216ae21"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "recipient": {
      "accountIdType": "ACC NO",
      "currency": "USD",
      "country": "AU",
      "email": "john@example.com"
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Query multiple recipients

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const bodyJSON = {
  variables: {
    input: {
      currency: "USD",
    },
  },
  query: `
query ($input: RecipientQueryInput!) {
  recipients(input: $input) {
    accountIdType currency country email
  } 
<strong>}`,
</strong>};
</code></pre>
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($input: RecipientQueryInput!) {
  recipients(input: $input) {
    accountIdType
    currency
    country
    email
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
{
  "input": { 
    "currency": "USD"
  }
}  
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "recipients": [
      {
        "accountIdType": "ACC NO",
        "currency": "USD",
        "country": "AU",
        "email": "john@example.com"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

#### Create a recipient

{% hint style="info" %}
In addresses the`suburb`field is an Australian suburb. For other countries you should put the city (e.g. Manila or London) or any other small administrative area name.

If you find it technically challenging to submit all components of the recipients’s address, we would appreciate it if you could at least collect the recipients’s country along with a complete address string that includes the postcode and put them into the `country` and `street` fields, respectively. In this case, you can skip the `postcode`, `suburb`, and `state` fields, and the recipient record will still be created.&#x20;


{% endhint %}

#### Create an Individual recipient

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      firstName: "John",
      lastName: "Malkovich",
      dob: "1987-06-05",
      accountIdType: "BSB",
      currency: "AUD",
      bsb: "370370",
      accountNo: "12341234",
      email: "john@example.com",
      address: {
        street: "22 Woolooware Rd",
        suburb: "Woolooware",
        state: "NSW",
        country: "AU",
        postcode: "2230",
      },
    },
  },
  query: `
mutation ($input: RecipientInput!) {
  createRecipient(input: $input) {
    success code message 
    recipient {
      id nickName accountIdType currency email
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: RecipientInput!) {
  createRecipient(input: $input) {
    success code message
    recipient {
      id nickName accountIdType currency email
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
    "firstName": "John",
    "lastName": "Malkovich",
    "dob": "1987-06-05",
    "accountIdType": "BSB",
    "currency": "AUD",
    "bsb": "370370",
    "accountNo": "12341234",
    "email": "john@example.com",
    "address": {
      "street": "22 Woolooware Rd",
      "suburb": "Woolooware",
      "state": "NSW",
      "country": "AU",
      "postcode": "2230"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createRecipient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Recipient created",
      "recipient": {
        "id": "6859c06eaa36ba8534d974f1",
        "nickName": "JohnMalkov",
        "accountIdType": "BSB",
        "currency": "AUD",
        "email": "john@example.com"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Create a Company recipient

{% tabs %}
{% tab title="JavaScript" %}
```graphql
const bodyJSON = {
  variables: {
    input: {
      companyName: "Acme Pty Ltd",
      accountIdType: "BSB",
      currency: "AUD",
      bsb: "370370",
      accountNo: "12341234",
      email: "john@example.com",
      address: {
        street: "22 Woolooware Rd",
        suburb: "Woolooware",
        state: "NSW",
        country: "AU",
        postcode: "2230",
      },
    },
  },
  query: `
mutation ($input: RecipientInput!) {
  createRecipient(input: $input) {
    success code message 
    recipient {
      id nickName accountIdType currency email
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: RecipientInput!) {
  createRecipient(input: $input) {
    success code message
    recipient {
      id nickName accountIdType currency email
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
    "companyName": "Acme Pty Ltd",
    "accountIdType": "BSB",
    "currency": "AUD",
    "bsb": "370370",
    "accountNo": "12341234",
    "email": "john@example.com",
    "address": {
      "street": "22 Woolooware Rd",
      "suburb": "Woolooware",
      "state": "NSW",
      "country": "AU",
      "postcode": "2230"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createRecipient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Recipient created",
      "recipient": {
        "id": "6859ca4baa36ba8534d97da1",
        "nickName": "Acme Pty L",
        "accountIdType": "BSB",
        "currency": "AUD",
        "email": "john@example.com"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Update recipient

{% hint style="info" %}
Please note the recipient's`accountIdType`can't be changed
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "5ba89a6b35a2b327b81ffc3b",
    input: {
      nickName: "JohnM",
      firstName: "John",
      lastName: "Malkovich",
      accountIdType: "BSB",
      currency: "AUD",
      bsb: "370370",
      accountNo: "12341234",
      email: "john@example.com",
      address: {
        street: "22 Woolooware Rd",
        suburb: "Woolooware",
        state: "NSW",
        country: "AU",
        postcode: "2230",
      },
    },
  },
  query: `
mutation ($id: ID, $input: RecipientInput!) {
  updateRecipient(id: $id, input: $input) {
    success code message  
    recipient {
      id nickName    
    }
  }
}`,
};  
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($id: ID, $input: RecipientInput!) {
  updateRecipient(id: $id, input: $input) {
    success
    code
    message
    recipient {
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
  "id": "5ba89a6b35a2b327b81ffc3b",
  "input":{
    "nickName": "JohnM",
    "firstName": "John",
    "lastName": "Malkovich",
    "accountIdType": "BSB",
    "currency": "AUD",
    "bsb": "370370",
    "accountNo": "12341234",
    "email": "john@example.com",
    "address": {
      "street": "22 Woolooware Rd",
      "suburb" : "Woolooware",
      "state": "NSW",
      "country": "AU",
      "postcode": "2230"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createRecipient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Recipient updated",
      "recipient": {
        "id": "5ba89a6b35a2b327b81ffc3b",
        "nickName": "JohnM"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Delete recipient

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "6b04c62ec0bf606bf216ae21",
  },
  query: `
mutation ($input: ID) {
  deleteRecipient(id: $input) {
    success code message
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: ID) {
  deleteRecipient(id: $input) {
    success 
    code 
    message
    # there are many other properties
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```graphql
{
  "input": "6b04c62ec0bf606bf216ae21"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "deleteRecipient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Recipient deleted"
    }
  }
}
```
{% endtab %}
{% endtabs %}

