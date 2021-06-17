---
description: Automatically receive and convert funds from other countries and currencies
---

# Auto receive funds

Some customers can automatically receive funds from overseas. Meaning, if we detect an overseas deposit to the FlashFX controlled bank account\(s\) then we can automatically create an inbound payment, convert funds, and top up your FlashFX balance with AUD.

{% hint style="info" %}
Note! You must send money from the **bank account on your \(or your sub-client's\) name**. Also, the payment must have the **payment reference** we told you. See below.
{% endhint %}

Here is how it looks step by step.

1. We would need to enable the foreign currency auto-receiving feature for you.
2. You, or your [Sub-client](../sub-clients.md), would have a special bank account in, say, SEPA zone. Find the details below.
   * The account number and **payment reference** depends on the currency and country you wish to deposit to. E.g. the EUR currency account is usually a British IBAN \(starts with "GB"\).
3. You, or your [Sub-client](../sub-clients.md), would deposit money to it. Make sure to submit the exact **payment reference** we told you! Otherwise, your funds will be returned.
4. The FlashFX would detect the account funding event and automatically create a EUR-&gt;AUD payment for you.
5. You would receive at least two webhook notifications - `payment_created` and `payment_complete`.
6. Your FlashFX AUD balance would increase accordingly.

To find which foreign currency bank account you would need to deposit to, please go to the [FlashConnect](https://connect.flash-fx.com/) and find there the list of inbound currencies we support and the corresponding bank account numbers. It's called the **"Funding Accounts"** throughout the user interface.

To find out the Funding Accounts via API please use the `fundingAccounts` query.

You should deposit your foreign currency to:

{% tabs %}
{% tab title="Query" %}
```graphql
{
  fundingAccounts(input: { currencies: [EUR, USD, HKD, CNY] }) {
    iban
    accountNo
    bic
    currency
    externalReference
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

Your [sub-clients](../sub-clients.md) should deposit their foreign currency to:

{% tabs %}
{% tab title="Query" %}
```graphql
{
  subClients(input: { externalId: "991188227733" }) {
    id
    externalId
    fundingAccounts(input: { currencies: [EUR, USD, CNY] }) {
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



