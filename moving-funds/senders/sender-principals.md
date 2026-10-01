---
description: >-
  Who owns and controls a company sender: UBOs, directors, secretaries and
  trustees
---

# Sender principals

For every `company` sender we also want to know who owns and controls it. These people are the sender's **principals**: its ultimate beneficial owners, beneficial owners, directors, company secretaries and trustees. Our banking partners require this information for every company on whose behalf funds are sent, and we pass it on to them with the payment.

Send them in the `principals` list of `createSender`. Each entry describes one person, exactly as for [sub-client principals](../../accounts/virtual-account-numbers/create-sub-clients/principals.md):

* `roles` - every role the person holds. Required, one or more of the values below
* `firstName` and `lastName` - required. Latin characters only, the same rules as for an individual sender's name
* `middleName` - optional
* `dob` - date of birth in `YYYY-MM-DD` format, optional

One person can hold several roles:

* `ULTIMATE_BENEFICIAL_OWNER` - owns or controls at least 25% of the organisation, or over 10% where it is registered, licensed or regulated in a high-risk jurisdiction.
* `BENEFICIAL_OWNER` - owns a share below the `ULTIMATE_BENEFICIAL_OWNER` threshold.
* `DIRECTOR` - a member of the board or equivalent governing body.
* `SECRETARY` - the company secretary.
* `TRUSTEE` - holds the organisation's assets on trust for its beneficiaries.
* `OTHER` - significant control or influence that none of the other roles describes.

Principals are accepted for `company` senders only: sending them for an individual is rejected with `INVALID_DATA`. Do not send an `id` when creating a sender - we assign one to every principal and return it in the response, so that you can [edit that person later](sender-principals.md#updating-sender-principals). Real people, real names, real dates of birth - the same data quality expectations apply as to the sender itself.

{% hint style="info" %}
Principals are optional for now. They will become mandatory for every `company` sender - we will announce the date in advance in the [API change log](../../basics/api-change-log.md). Please start sending them with every new company sender and add them to your existing ones via `updateSender`. The same details can be recorded in Flash Connect.&#x20;
{% endhint %}

You can read them back at any time via the `principals` field of the `Sender` type (`id`, `roles`, `firstName`, `middleName`, `lastName`, `dob`). It is an empty list for individual senders.

{% hint style="warning" %}
Principals live on sender records only. A `sender` object submitted inline to [`createWithdrawal`](../payouts/withdraw-funds.md#sender-sender-object-senderid-subclientid-or-neither) or [`createPayment`](../../fx/payments/send-funds.md#sender-senderid-or-subclientid-or-neither) is not saved as a sender, so any `principals` in it are ignored. To attach principals to the sender of a withdrawal or FX payment, create the sender with `createSender` first and pass its `senderId`. The `sender` of such a withdrawal or payment then returns the principals too; for an inline sender, `principals` is `null`. For withdrawals and payments made on behalf of a sub-client, read the principals from the [sub-client](../../accounts/virtual-account-numbers/create-sub-clients/principals.md) instead.
{% endhint %}

#### Creating a company sender with principals

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      companyName: "Acme Pte Ltd",
      website: "acme.com",
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
      principals: [
        {
          firstName: "Olivia",
          lastName: "Hart",
          dob: "1978-11-23",
          roles: ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"],
        },
        {
          firstName: "Samuel",
          middleName: "James",
          lastName: "Reyes",
          roles: ["BENEFICIAL_OWNER"],
        },
      ],
    },
  },
  query: `
mutation ($input: SenderInput!) {
  createSender(input: $input) {
    success code message
    sender {
      id nickName
      principals {
        id roles firstName middleName lastName dob
      }
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
    success
    code
    message
    sender {
      id
      nickName
      principals {
        id
        roles
        firstName
        middleName
        lastName
        dob
      }
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
    "website": "acme.com",
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
    },
    "principals": [
      {
        "firstName": "Olivia",
        "lastName": "Hart",
        "dob": "1978-11-23",
        "roles": ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"]
      },
      {
        "firstName": "Samuel",
        "middleName": "James",
        "lastName": "Reyes",
        "roles": ["BENEFICIAL_OWNER"]
      }
    ]
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
        "id": "68db5f0c1a2b3c4d5e6f7a81",
        "nickName": "Acme Pte L",
        "principals": [
          {
            "id": "68db5f0c1a2b3c4d5e6f7a91",
            "roles": ["DIRECTOR", "ULTIMATE_BENEFICIAL_OWNER"],
            "firstName": "Olivia",
            "middleName": null,
            "lastName": "Hart",
            "dob": "1978-11-23"
          },
          {
            "id": "68db5f0c1a2b3c4d5e6f7a92",
            "roles": ["BENEFICIAL_OWNER"],
            "firstName": "Samuel",
            "middleName": "James",
            "lastName": "Reyes",
            "dob": null
          }
        ]
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Updating sender principals

`principals` is the **complete list** of the people who own or control a `company` sender. What you send replaces what is on record:

* an entry with an `id` - that principal is updated
* an entry without an `id` - a new principal is added
* a principal on record whose `id` you do not send - that principal is removed and is no longer returned

Every entry needs the person's full details (`roles`, `firstName`, `lastName`, plus `middleName` and `dob` if you have them), even when you only change their roles. Optional fields you leave out are kept as they are. The list cannot be left with no one: omitting `principals`, or sending an empty list, leaves the principals unchanged. An `id` that does not belong to this sender is rejected with `NOT_FOUND`, and principals on an individual sender are rejected with `INVALID_DATA`.

`updateSender` always needs the sender's `companyName` and `address` alongside `principals`; the other sender fields you leave out are kept.

In the example below the sender had two principals on record: Olivia Hart (director and ultimate beneficial owner) and Samuel Reyes (beneficial owner). The request keeps Olivia as a director only, adds Priya Nair as the company secretary, and removes Samuel because his `id` is not in the list.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    id: "68db5f0c1a2b3c4d5e6f7a81",
    input: {
      companyName: "Acme Pte Ltd",
      address: {
        street: "1 Test St",
        suburb: "London",
        state: "TST",
        country: "GB",
        postcode: "2000",
      },
      principals: [
        {
          id: "68db5f0c1a2b3c4d5e6f7a91",
          firstName: "Olivia",
          lastName: "Hart",
          dob: "1978-11-23",
          roles: ["DIRECTOR"],
        },
        {
          firstName: "Priya",
          lastName: "Nair",
          roles: ["SECRETARY"],
        },
      ],
    },
  },
  query: `
mutation ($id: ID, $input: SenderInput!) {
  updateSender(id: $id, input: $input) {
    success code message
    sender {
      id
      principals {
        id roles firstName middleName lastName dob
      }
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($id: ID, $input: SenderInput!) {
  updateSender(id: $id, input: $input) {
    success
    code
    message
    sender {
      id
      principals {
        id
        roles
        firstName
        middleName
        lastName
        dob
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "id": "68db5f0c1a2b3c4d5e6f7a81",
  "input": {
    "companyName": "Acme Pte Ltd",
    "address": {
      "street": "1 Test St",
      "suburb": "London",
      "state": "TST",
      "country": "GB",
      "postcode": "2000"
    },
    "principals": [
      {
        "id": "68db5f0c1a2b3c4d5e6f7a91",
        "firstName": "Olivia",
        "lastName": "Hart",
        "dob": "1978-11-23",
        "roles": ["DIRECTOR"]
      },
      {
        "firstName": "Priya",
        "lastName": "Nair",
        "roles": ["SECRETARY"]
      }
    ]
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
      "code": "UPDATED",
      "message": "Sender 68db5f0c1a2b3c4d5e6f7a81 updated.",
      "sender": {
        "id": "68db5f0c1a2b3c4d5e6f7a81",
        "principals": [
          {
            "id": "68db5f0c1a2b3c4d5e6f7a91",
            "roles": ["DIRECTOR"],
            "firstName": "Olivia",
            "middleName": null,
            "lastName": "Hart",
            "dob": "1978-11-23"
          },
          {
            "id": "68db5f0c1a2b3c4d5e6f7a93",
            "roles": ["SECRETARY"],
            "firstName": "Priya",
            "middleName": null,
            "lastName": "Nair",
            "dob": null
          }
        ]
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Reading the principals of a sender

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "68db5f0c1a2b3c4d5e6f7a81",
  },
  query: `
query ($input: ID) {
  sender(id: $input) {
    id companyName
    principals {
      id roles firstName middleName lastName dob
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
    id
    companyName
    principals {
      id
      roles
      firstName
      middleName
      lastName
      dob
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": "68db5f0c1a2b3c4d5e6f7a81"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "sender": {
      "id": "68db5f0c1a2b3c4d5e6f7a81",
      "companyName": "Acme Pte Ltd",
      "principals": [
        {
          "id": "68db5f0c1a2b3c4d5e6f7a91",
          "roles": ["DIRECTOR"],
          "firstName": "Olivia",
          "middleName": null,
          "lastName": "Hart",
          "dob": "1978-11-23"
        },
        {
          "id": "68db5f0c1a2b3c4d5e6f7a93",
          "roles": ["SECRETARY"],
          "firstName": "Priya",
          "middleName": null,
          "lastName": "Nair",
          "dob": null
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

`principals` is available wherever a `Sender` is returned, including the `senders` list and the `sender` of a withdrawal or FX payment, but it is looked up per sender, so request it only where you need it.

