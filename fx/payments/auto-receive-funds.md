---
description: Automatically receive and convert funds from other countries and currencies
---

# Auto receive funds

Some customers can automatically receive funds from overseas. Meaning, if we detect an overseas deposit to the Flash Payments controlled bank account(s) then we can automatically create an inbound payment, convert funds, and top up your Flash Payments balance with AUD.

{% hint style="warning" %}
Please note that funds must be transferred from a bank account registered under your company name. If a virtual Flash sub-account is used as the beneficiary, the transfer should be made from an account in your sub-client’s name.\
\
Whenever a sub-client record is involved in the transaction, their full real-world address is always required. For your local Australian sub-clients, please ensure all address components are provided when adding a sub-client to our system.\
\
In some cases, but not always, the payment must include the specific payment reference we provide. See below for more details.
{% endhint %}

Here is how it looks step by step.

1. We would need to enable the foreign currency auto-receiving feature for you.
2. You, or your [Sub-client](../../accounts/virtual-account-numbers/), would have a special bank account in, say, SEPA zone. Find the details below.
   * The account number and **payment reference** depends on the currency and country you wish to deposit to. E.g. the EUR currency account is usually a British IBAN (starts with "GB").
3. You, or your [Sub-client](../../accounts/virtual-account-numbers/), would deposit money to it. Make sure to submit the exact **payment reference** we told you! Otherwise, your funds will be returned.
4. The Flash Payments would detect the account funding event and automatically create a EUR->AUD payment for you.
5. You would receive at least two webhook notifications - `payment_created` and `payment_complete`.
6. Your Flash Payments AUD balance would increase accordingly.

### &#x20;Funding Accounts&#x20;

\
To find which foreign currency bank account you need to deposit into, please visit [FlashConnect](https://connect.uat.flash-payments.com.au/) and locate the list of supported inbound currencies and their corresponding bank account numbers. It is referred to as the **“Funding Accounts”** throughout the user interface.

{% hint style="info" %}
You can simulate and test an international inbound payment with the FlashConnect tool in the UAT environment. Just go to the _FX Payments_ page and click "SEND TEST INBOUND PAYMENT".\
\
Additionally, you can test an international inbound payment sent by your [sub-client](../../accounts/virtual-account-numbers/) in the UAT. Just go to the _Sub-clients_ page, find the sub-client, and click "SEND TEST INBOUND PAYMENT".
{% endhint %}

To find out the Funding Accounts via API please use the `fundingAccounts` query.

You should deposit your foreign currency to one of the following master accounts:

{% tabs %}
{% tab title="JavaScript" %}
```graphql
const bodyJSON = {
  variables: {
    input: {
      currencies: ["EUR","USD","HKD","CNY"],
      },
  },
  query: `
query ($input: FundingAccountQueryInput!) {
  fundingAccounts(input: $input) {
    iban accountNo accountName accountAddress bic currency externalReference
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
  query($input: FundingAccountQueryInput!) {
  fundingAccounts(input: $input) {
    iban
    accountNo
    accountName
    accountAddress
    bic
    currency
    externalReference
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
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
    "fundingAccounts": [
      {
        "iban": "GB91 BARC 2006 0565 4685 66",
        "accountNo": "65468566",
        "bic": "BARCGB22",
        "currency": "USD",
        "externalReference": "191127-99999"
      },
      {
        "iban": "GB05 BARC 2006 0574 7412 77",
        "accountNo": "74741277",
        "bic": "BARCGB22",
        "currency": "EUR",
        "externalReference": "191127-99999"
      },
      {
        "iban": null,
        "accountNo": "87135588",
        "bic": "BARCGB22",
        "currency": "CNY",
        "externalReference": "191127-99999"
      },
      {
        "iban": "GB87 BARC 2006 0546 9946 00",
        "accountNo": "46994600",
        "bic": "BARCGB22",
        "currency": "HKD",
        "externalReference": "191127-99999"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

Your [sub-clients](../../accounts/virtual-account-numbers/) should deposit their foreign currency to one of the follwing virtual sub-accounts:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    sc: {
      externalId: "991188227733",
    },
    fa: {
      currencies: ["EUR","USD","CNY"],
    },
  },
  query: `
query ($sc: SubClientQueryInput!, $fa: FundingAccountQueryInput!) {
  subClients(input: $sc) {
    id externalId 
    fundingAccounts(input: $fa) {
      iban accountNo accountName accountAddress bic currency externalReference
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query($sc: SubClientQueryInput!, $fa: FundingAccountQueryInput!) {
  subClients(input: $sc) {
    id
    externalId
    fundingAccounts(input: $fa) {
      iban
      accountNo
      accountName
      accountAddress
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
  "sc": { 
    "externalId": "991188227733" 
  },
  "fa": {
    "currencies": ["EUR", "USD", "CNY"] 
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
        "id": "60a1e9e76eaedbf66964a323",
        "externalId": "991188227733",
        "fundingAccounts": [
          {
            "iban": "GB91 BARC 2006 0565 4685 66",
            "accountNo": "65468566",
            "bic": "BARCGB22",
            "currency": "USD",
            "externalReference": "210616-99999"
          },
          {
            "iban": "GB05 BARC 2006 0574 7412 77",
            "accountNo": "74741277",
            "bic": "BARCGB22",
            "currency": "EUR",
            "externalReference": "210616-99999"
          },
          {
            "iban": null,
            "accountNo": "87135588",
            "bic": "BARCGB22",
            "currency": "CNY",
            "externalReference": "210616-99999"
          },
        ]
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}
