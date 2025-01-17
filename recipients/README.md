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

#### Query single recipient

{% tabs %}
{% tab title="Query" %}
```graphql
{
  recipient(id: "12341234123412341234") {
    accountIdType
    currency
    country
    email
    # there are many other properties
  }
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
{% tab title="Query" %}
```graphql
{
  recipients(input: { currency: USD }) {
    accountIdType
    currency
    country
    email
    # there are many other properties
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

{% tabs %}
{% tab title="Individual" %}
```graphql
mutation {
  createRecipient(
    input: {
      firstName: "John"
      lastName: "Malkovich"
      dob: "1987-06-05"
      accountIdType: BSB
      currency: AUD
      bsb: "123456"
      accountNo: "12341234"
      email: "john@example.com"
      address: {
        street: "22 Woolooware Rd"
        suburb: "Woolooware"
        state: "NSW"
        country: AU
        postcode: "2230"
      }
    }
  ) {
    success
    code
    message
    recipient {
      id
      nickName
      accountIdType
      currency
      email
      # there are many other properties
    }
  }
}
```
{% endtab %}

{% tab title="Company/Corporate" %}
```graphql
mutation {
  createRecipient(
    input: {
      companyName: "Acme Pty Ltd"
      accountIdType: BSB
      currency: AUD
      bsb: "370370"
      accountNo: "123412340"
      email: "acme@example.com"
      address: {
        street: "22 Woolooware Rd"
        suburb: "Woolooware"
        state: "NSW"
        country: AU
        postcode: "2230"
      }
    }
  ) {
    success
    code
    message
    recipient {
      id
      nickName
      accountIdType
      currency
      email
      # there are many other properties
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
{% tab title="Query" %}
```graphql
mutation {
  updateRecipient(
    id: "5ba89a6b35a2b327b81ffc3b",
    input: {
      nickName: "JohnM"
    
      firstName: "John"
      lastName: "Malkovich"
      accountIdType: BSB
      currency: AUD
      bsb: "123456"
      accountNo: "12341234"
      email: "john@example.com"
      address: {
        street: "22 Woolooware Rd"
        suburb: "Woolooware"
        state: "NSW"
        country: AU
        postcode: "2230"
      }
    }
  ) {
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
{% tab title="Query" %}
```graphql
mutation {
  deleteRecipient(id: "5ba89a6b35a2b327b81ffc3b") {
    success code message
  }
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

