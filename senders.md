---
description: CRUD queries for your payment senders
---

# Senders

We are legally obliged to collect the actual sender details. Please, do not send us an intermediate organisation details such as exchanges, banks, gateways, etc.

If receiving from yourself then please provide your own details. See the DOCS in [Playground](https://api.flash-payments.com/) for other sender details options.

* `sender` and `senders` queries - **read** your address book.
* `createSender` - **creates** a new record in the Flash Payments database.
* `updateSender` - **updates** an existing sender.
* `deleteSender` - **deletes** an existing sender.

#### Query single sender

{% tabs %}
{% tab title="Query" %}
```graphql
{
  sender(id: "59f2733f2519e236edab0efe") {
    email
    firstName
    lastName
    companyName
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
{% tab title="Query" %}
```graphql
{
  senders(input: { email: "john@example.com" }) {
    email
    firstName
    lastName
    companyName
    address {
      country
    }
    # there are other properties
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

If you find it technically challenging to submit all components of the sender’s address, we would appreciate it if you could at least collect the sender’s country along with a complete address string that includes the postcode and put them into the `country` and `street` fields, respectively. In this case, you can skip the `postcode`, `suburb`, and `state` fields, and the sender record will still be created.&#x20;
{% endhint %}

{% hint style="info" %}
The date of birth (`dob`) is not required. However, if you don't have it then your transactions will trigger more compliance alerts, thus your payments will be processed much slower (hours or days instead of mere seconds). Also, we charge you more fees for manual labour you cause.
{% endhint %}

{% tabs %}
{% tab title="Individual" %}
<pre class="language-graphql"><code class="lang-graphql"><strong>mutation {
</strong>  createSender(
    input: {
      firstName: "Malcolm"
      lastName: "Jez"
      dob: "2000-01-01"
      email: "malcolm@example.com"
      mobile: "+1 123412341234"
      address: {
        street: "1 Test St"
        suburb: "London"
        state: "TST"
        country: GB
        postcode: "2000"
      }
      idDoc: {
        type: passport
        docNumber: "GB1234321"
        issuer: "GB"
      }
    }
  ) {
    success
    code
    message
    sender {
      id
      nickName
      # there are many other properties
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Company/Corporate" %}
<pre class="language-graphql"><code class="lang-graphql"><strong>mutation {
</strong>  createSender(
    input: {
      companyName: "Acme Pte Ltd"
      businessNumber: "12345678912"
      email: "acme@example.com"
      mobile: "+1 123412341234"
      address: {
        street: "1 Test St"
        suburb: "London"
        state: "TST"
        country: GB
        postcode: "2000"
      }
      idDoc: {
        type: passport
        docNumber: "GB1234321"
        issuer: "GB"
      }
    }
  ) {
    success
    code
    message
    sender {
      id
      nickName
      # there are many other properties
    }
  }
}
</code></pre>
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
      mobile: "+1 123412341234"
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
        issuer: "GB"
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

