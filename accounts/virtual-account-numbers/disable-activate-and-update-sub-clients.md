# Disable, Activate and Update sub-clients

You can disable and activate sub-clients. Deposits sent to a disabled sub-client will no longer be booked against your balance.

#### Disabling a sub-client

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "606128f24bf29139b2cf74ef",
  },
  query: `
mutation ($input: ID!) {
  disableSubClient(id: $input) {
    success code message 
    subClient {
      id status
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: ID!) {
  disableSubClient(id: $input) {
    success
    code
    message
    subClient {
      id
      status
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
   "input": "606128f24bf29139b2cf74ef"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "disableSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully disabled",
      "subClient": {
        "id": "606128f24bf29139b2cf74ef",
        "status": "DISABLED"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Activating a sub-client

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: "606128f24bf29139b2cf74ef",
  },
  query: `
mutation ($input: ID!) {
  activateSubClient(id: $input) {
    success code message 
    subClient {
      id status
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GrraphQL Query" %}
```graphql
mutation($input: ID!) {
  activateSubClient(id: $input) {
    success
    code
    message
    subClient {
      id
      status
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
   "input": "606128f24bf29139b2cf74ef"
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "activateSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully activated",
      "subClient": {
        "id": "606128f24bf29139b2cf74ef",
        "status": "ACTIVE"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Updating sub-clients

At this point, you can only update the `externalId`, `address`, `postalAddress` and `principals` properties because each sub-client has a set of linked domestic and international Virtual Account Numbers to send and receive funds.

{% hint style="info" %}
Please note, that the updated address will be re-verified, so make sure to include all its components, even if some fields, like the `country`, remain unchanged.<br>
{% endhint %}

#### Updating sub-client externalId

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = { 
  variables: {
    id: "660fef8e1f3b5452bd6945ec",
    input: {
      externalId: "my_system_id_29f-ae0978b00d09e",
    },
  }, 
  query: `
mutation ($id: ID!, $input: UpdateSubClientInput!) {
  updateSubClient(id: $id, input: $input) {
    success code message
    subClient {
      id status externalId
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($id: ID!, $input: UpdateSubClientInput!) {
  updateSubClient(id: $id, input: $input) {
    success
    code
    message
    subClient {
      id
      status
      externalId
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
  "id": "660fef8e1f3b5452bd6945ec", 
  "input": {
    "externalId": "my_system_id_29f-ae0978b00d09e"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "updateSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully updated",
      "subClient": {
        "id": "660fef8e1f3b5452bd6945ec",
        "status": "ACTIVE",
        "externalId": "my_system_id_29f-ae0978b00d09e"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The same [uniqueness rule](create-sub-clients/#duplicate-sub-clients-and-externalid) applies when updating: if the new `externalId` is already used by another of your sub-clients, the update is rejected with `DUPLICATE_SUBCLIENT`, and the sub-client is left unchanged. Re-sending a sub-client's own current `externalId` is accepted.
{% endhint %}

#### Updating sub-client address

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = { 
  variables: {
    id: "660fef8e1f3b5452bd6945ec",
    input: {
      adddress: {
        street: "456 New St",
        suburb: "Newtow",
        state: "VIC",
        postcode: "3220", 
        country: "AU",
      },
    },
  }, 
  query: `
mutation ($id: ID!, $input: UpdateSubClientInput!) {
  updateSubClient(id: $id, input: $input) {
    success code message
    subClient {
      id status externalId address {building street suburb state postcode country}
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($id: ID!, $input: UpdateSubClientInput!) {
  updateSubClient(id: $id, input: $input) {
    success
    code
    message
    subClient {
      id
      status
      externalId
      address {
        building 
        street
        suburb
        state
        postcode
        country
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
  "id": "660fef8e1f3b5452bd6945ec", 
  "input": {
    "address": {
        "street": "456 New St",
        "suburb": "Newtow",
        "state": "VIC",
        "postcode": "3220",
        "country": "AU"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "updateSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully updated",
      "subClient": {
        "id": "660fef8e1f3b5452bd6945ec",
        "status": "ACTIVE",
        "externalId": "my_system_id_29f-ae0978b00d09e",
        "address": {
          "building": null,
          "street": "456 New St",
          "suburb": "Newtow",
          "state": "VIC",
          "postcode": "3220",
          "country": "AU"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

#### Updating sub-client principals

`principals` is the **complete list** of the people who own or control a `company` sub-client. What you send replaces what is on record:

* an entry with an `id` - that principal is updated
* an entry without an `id` - a new principal is added
* a principal on record whose `id` you do not send - that principal is removed and is no longer returned

Every entry needs the person's full details (`roles`, `firstName`, `lastName`, plus `middleName` and `dob` if you have them), even when you only change their roles. Optional fields you leave out are kept as they are. The list cannot be left with no one: omitting `principals`, or sending an empty list, leaves the principals unchanged. An `id` that does not belong to this sub-client is rejected with `NOT_FOUND`, and principals on an `individual` sub-client are rejected with `INVALID_DATA`.&#x20;

In the example below the sub-client had two principals on record: Wei Zhang (`68d4c3a1f2b9e07c4d1a5b01`, director and ultimate beneficial owner) and Mei Lin (`68d4c3a1f2b9e07c4d1a5b02`, beneficial owner). The request keeps Wei as a director only, adds Carol Nguyen as the company secretary, and removes Mei because her `id` is not in the list.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = { 
  variables: {
    id: "660fef8e1f3b5452bd6945ec",
    input: {
      principals: [
        {
          id: "68d4c3a1f2b9e07c4d1a5b01",
          firstName: "Wei",
          lastName: "Zhang",
          dob: "1975-03-02",
          roles: ["DIRECTOR"],
        },
        {
          firstName: "Carol",
          lastName: "Nguyen",
          roles: ["SECRETARY"],
        },
      ],
    },
  }, 
  query: `
mutation ($id: ID!, $input: UpdateSubClientInput!) {
  updateSubClient(id: $id, input: $input) {
    success code message
    subClient {
      id status principals { id roles firstName middleName lastName dob }
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($id: ID!, $input: UpdateSubClientInput!) {
  updateSubClient(id: $id, input: $input) {
    success
    code
    message
    subClient {
      id
      status
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
  "id": "660fef8e1f3b5452bd6945ec", 
  "input": {
    "principals": [
      {
        "id": "68d4c3a1f2b9e07c4d1a5b01",
        "firstName": "Wei",
        "lastName": "Zhang",
        "dob": "1975-03-02",
        "roles": ["DIRECTOR"]
      },
      {
        "firstName": "Carol",
        "lastName": "Nguyen",
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
    "updateSubClient": {
      "success": true,
      "code": "SUCCESS",
      "message": "Sub-client was successfully updated",
      "subClient": {
        "id": "660fef8e1f3b5452bd6945ec",
        "status": "ACTIVE",
        "principals": [
          {
            "id": "68d4c3a1f2b9e07c4d1a5b01",
            "roles": ["DIRECTOR"],
            "firstName": "Wei",
            "middleName": null,
            "lastName": "Zhang",
            "dob": "1975-03-02"
          },
          {
            "id": "68d5e0b7c1a2d93f4e6b7c03",
            "roles": ["SECRETARY"],
            "firstName": "Carol",
            "middleName": null,
            "lastName": "Nguyen",
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
