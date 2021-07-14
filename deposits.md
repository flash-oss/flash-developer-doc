---
description: Explains how to accept deposits programmatically
---

# Deposits

After fully registering with us, you get a BSB and a dedicated bank account number.

{% hint style="warning" %}
Warning: The account number can only process local transfers, **no SWIFT/RTGS**.
{% endhint %}

Every deposit to this account would increase your FlashFX balance.

You can receive notifications via [Webhooks](webhooks/webhooks.md) about every deposit.

The deposit data includes the payment reference \(we call it `externalReference` in this API\).

By default, only yourself is allowed to deposit. However, we can enable third party deposits feature for your account. This, effectively, makes your account number a local collection account.

We can also enable the [sub-client](sub-clients.md) feature. It allows you to programmatically create client accounts for transaction purposes, with dedicated BSB and account number. You can give these bank account details to your clients to accept deposits. Once funds arrive, we will increase your account balance, and you will see a deposit with sub-client information linked to it.

To browse your deposits, you can use our FlashConnect tool: [https://connect.flash-fx.com/](https://connect.flash-fx.com/)

{% hint style="info" %}
Tip. You can simulate a deposit using the FlashConnect tool. Just go to the _Deposits_ page and click "SEND TEST DEPOSIT". It's available only in our development environment.  
Additionally, you can fake a deposit sent by your [sub-client](sub-clients.md). Just go to the _Sub-clients_ page, find the sub-client, and click "SEND TEST DEPOSIT". It's available only in our development environment.
{% endhint %}

### Querying deposits

#### Query all deposits

{% tabs %}
{% tab title="Request" %}
```graphql
{
  deposits {
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
{% tab title="Query" %}
```graphql
{
  deposit(id: "6053d4e0e3bc655e0598a742") {
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

