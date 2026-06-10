---
description: Retrieve the members of your client account
---

# Account members

The read-only `members` query returns a list of registered users for your client account, including their contact details, roles, access controls, and account status. This provides a programmatic way to retrieve the list of your account users and their information, which can otherwise be obtained via the Flash Connect portal.

The output is always limited to your client account, meaning you can only see your own members.

### Query all members

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
    },
  },
  query: `
query ($input: MembersQueryInput) {
  members(input: $input) {
    id firstName middleName lastName dob email mobile
    address {
      building streetNo street suburb state postcode country
    }
    status roles access isPrimaryContact
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query ($input: MembersQueryInput) {
  members(input: $input) {
    id
    firstName
    middleName
    lastName
    dob
    email
    mobile
    address {
      building
      streetNo
      street
      suburb
      state
      postcode
      country
    }
    status
    roles
    access
    isPrimaryContact
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 {
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "members": [
      {
        "id": "5fb314cb9224595df522db61",
        "firstName": "John",
        "middleName": null,
        "lastName": "Smith",
        "dob": "1980-12-12",
        "email": "john.smith@example.com",
        "mobile": "+61 422 832 849",
        "address": {
          "building": null,
          "streetNo": "25",
          "street": "Moore St",
          "suburb": "Waterloo",
          "state": "NSW",
          "postcode": "2017",
          "country": "AU"
        },
        "status": "ACTIVE",
        "roles": ["director", "treasurer"],
        "access": ["admin", "transact", "approve"],
        "isPrimaryContact": true
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

### Filtering members

`MembersQueryInput` accepts three optional list filters: `status`, `roles` and `access`. Omit a field to leave it unfiltered. A member matches a list filter if it has **at least one** of the listed values.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      status: ["ACTIVE"],
      roles: ["director"],
      access: ["transact"],
    },
  },
  query: `
query ($input: MembersQueryInput) {
  members(input: $input) {
    id firstName lastName status roles access
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
query ($input: MembersQueryInput) {
  members(input: $input) {
    id
    firstName
    lastName
    status
    roles
    access
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{
  "input": {
    "status": ["ACTIVE"],
    "roles": ["director"],
    "access": ["transact"]
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "members": [
      {
        "id": "5fb314cb9224595df522db61",
        "firstName": "John",
        "lastName": "Smith",
        "status": "ACTIVE",
        "roles": ["director"],
        "access": ["transact", "login"]
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

### Member statuses

| Member Status       | Meaning                                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------------------- |
| `ACTIVE`            | Fully verified and able to log in and transact.                                                       |
| `REGISTERING`       | Has started registration but not finished it yet.                                                     |
| `PASSWORD_REQUIRED` | Created without a password and must set one before logging in.                                        |
| `UNAPPROVED`        | Has finished registration and is waiting to be verified.                                              |
| `SUSPENDED`         | Cannot log in or transact. Covers accounts that are inactive, locked, failed verification or deleted. |

### Member Roles

The roles are descriptive and identify a member’s position within the client organisation. They do not, by themselves, grant access (access is regulated by the access controls below). One member can hold several roles.

| Member Role   | Meaning                                                      |
| ------------- | ------------------------------------------------------------ |
| `director`    | A director of the company.                                   |
| `partner`     | A partner of the partnership.                                |
| `secretary`   | A company secretary.                                         |
| `beneficiary` | A beneficial owner of the client.                            |
| `treasurer`   | Manages the client's funds and finances.                     |
| `engineer`    | A technical contact, e.g. someone integrating with this API. |
| `support`     | A support contact for day-to-day queries.                    |
| `compliance`  | A compliance contact for the client.                         |

### Member Access

The Access Control Levels (ACLs) define what a member is permitted to do on the account. A member can hold several ACLs.

| Member ACL       | Grants                                                         |
| ---------------- | -------------------------------------------------------------- |
| `admin`          | Full administrative control, including managing other members. |
| `transact`       | Create and send payments and conversions.                      |
| `login`          | Log in to the account.                                         |
| `api`            | Authenticate and use this API on behalf of the client.         |
| `correspondence` | Receive account correspondence and notifications.              |
| `approve`        | Approve payments that require a second authorisation.          |
| `compliance`     | Access compliance-related information and tasks.               |
