---
description: >-
  Give each sub-client a PayID at your own domain and collect deposits addressed
  to it
---

# Sub-client PayIDs

PayIDs are easy-to-remember aliases for Virtual Account Numbers (VANs) that your sub-clients can use to pay you instead of their Australian BSB and account numbers.&#x20;

### Domain PayIDs

You can register a PayID on your sub-client virtual account with Flash using your approved email domain, such as `jane.smith@yourdomain.com.au`. This allows you to accept domestic AUD transfers from your customer by that alias within Australia, as demonstrated in the examples below. A transfer sent to a PayID arrives as an ordinary deposit on the sub-client’s VAN and increases your master account balance, so deposit processing remains unchanged.

{% hint style="info" %}
PayID registration is disabled by default. Contact us to enable it and to approve the email domain (or domains) you will register PayIDs at.&#x20;
{% endhint %}

#### What you can and cannot register

* An email address at a domain approved for you. The domain has to be yours, typically the one you issue customer mailboxes at. Public email providers cannot be approved. The part before the `@` is also yours to choose, for example, it can be a customer name or another reference.&#x20;
* The sub-client's personal email address or an individual phone number are currently not allowed to be used as PayIDs. Contact us if your integration needs them.
* PayIDs based on an ABN or an organisation's name are not supported at the moment.

{% hint style="warning" %}
A PayID can be registered with one VAN only and we never disclose who holds a taken PayID. Please always register a PayID on the virtual account of the sub-client it belongs to.
{% endhint %}

#### Registering a PayID

Register the PayID for a sub-client with `registerAlias`. In this example, `example.com` is a domain approved for you.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      alias: "acme-corp@example.com",
      subClientId: "606d28675a2d931bc925fec2",
    },
  },
  query: `
mutation ($input: RegisterAliasInput!) {
  registerAlias(input: $input) {
    success code message
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: RegisterAliasInput!) {
  registerAlias(input: $input) {
    success
    code
    message
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "alias": "acme-corp@example.com",
    "subClientId": "606d28675a2d931bc925fec2"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "registerAlias": {
      "success": true,
      "code": "ALIAS_REGISTERED",
      "message": "The alias is registered and can receive payments."
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Registration completes within the request. There is no webhook to wait for. When the reply is `ALIAS_REGISTERED`, the PayID is live and can receive payments immediately.
{% endhint %}

#### **Listing a sub-client's PayIDs**

The `aliases` field of the `SubClient` type lists the sub-client's PayIDs, newest first, pending ones included. `aliases` is resolved separately for every sub-client in a response, so request it only where you need it. Deregistered PayIDs are not listed.&#x20;

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "606d28675a2d931bc925fec2",
  },
  query: `
query ($id: ID!) {
  subClient(id: $id) {
    id fullName
    aliases {
      alias aliasType status createdAt
    }
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
    aliases {
      alias
      aliasType
      status
      createdAt
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "id": "606d28675a2d931bc925fec2"
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
      "aliases": [
        {
          "alias": "acme-corp-invoices@example.com",
          "aliasType": "EMAIL",
          "status": "ACTIVE",
          "createdAt": "2026-10-08T03:41:07.219Z"
        },
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

`status` is one of:

* `ACTIVE` - registered and able to receive payments
* `PENDING` - not yet able to receive payments: briefly while a registration completes
* `DISABLED` - temporarily disabled; please contact us
* `PORTED` - being transferred to another financial institution

#### **Deregistering a PayID**

`deregisterAlias` releases the PayID: it stops receiving payments immediately and the alias becomes available for registration elsewhere.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      alias: "acme-corp@example.com",
    },
  },
  query: `
mutation ($input: DeregisterAliasInput!) {
  deregisterAlias(input: $input) {
    success code message
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: DeregisterAliasInput!) {
  deregisterAlias(input: $input) {
    success
    code
    message
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "alias": "acme-corp@example.com"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "deregisterAlias": {
      "success": true,
      "code": "ALIAS_DEREGISTERED",
      "message": "Alias deregistered"
    }
  }
}
```
{% endtab %}
{% endtabs %}

A deregistered PayID can be registered again later, and that is a new registration. When you deactivate a sub-client, its PayIDs are deregistered together with its VAN, and activating it again does not bring them back.

#### Receiving payments to a PayID

Customers pay their sub-client VAN by the PayID instead of the BSB and account number. Before paying, their bank shows them their sub-client's account name strictly as registered with Flash (it cannot be customized). The payment arrives as an ordinary deposit on the sub-client's virtual account, with the PayID recorded on it. See Deposits to a PayID.

#### PayID Limits

To protect the PayID scheme, each sub-client can hold a small number of PayIDs, and your account has a daily allowance of new registrations across all your sub-clients. Every attempt to register a new PayID counts towards it, whether or not it succeeds. We do not disclose the specific figures for the these limits here for security reasons. Contact us if these allowances do not fit your use case.

#### Testing in UAT

{% hint style="info" %}
PayIDs registered in UAT exist only in our simulation, they are not registered with the PayID scheme.
{% endhint %}

You can run the UAT simulation for any address at your approved domain and exercise registration, listing and deregistration freely. The permissions above still apply, so ask us to enable PayID registration and approve your domain on your UAT account as well.

The deposit details in Flash Connect show the PayID as well. To simulate a payment to a PayID, open the Sub-clients page in [Flash Connect](https://connect.uat.flash-payments.com.au/login) UAT, find the sub-client, click "SEND TEST DEPOSIT" and pick one of its PayIDs. The deposit arrives with that PayID on its `recipient`.&#x20;
