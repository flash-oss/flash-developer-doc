---
description: CRUD queries for your payment recipients
---

# Recipients

We are legally obliged to collect the actual recipient details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

Please, send us the final funds recipient. If sending to self then please provide your own details. See the DOCS in [Playground](https://api.flash-fx.com/) for other recipient details options.

* `recipient` and `recipients` queries - **read** your address book.
* `createRecipient` - **creates** a new record in the FlashFX database.
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
      "accountIdType": "RIPPLE",
      "currency": "XRP",
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
  recipients(input: { currency: XRP }) {
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
        "accountIdType": "RIPPLE",
        "currency": "XRP",
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
In addresses the `suburb` is an Australian suburb. For other countries you should put the city \(e.g. Manila or London\) or any other small administrative area name.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createRecipient(
    input: {
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
      accountIdType
      currency
      email
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
      "message": "Recipient created",
      "recipient": {
        "id": "5ba89a6b35a2b327b81ffc3b",
        "nickName": "JohnMalkov",
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



