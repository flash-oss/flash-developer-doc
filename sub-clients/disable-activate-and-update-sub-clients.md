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

At this point, you can update the `externalId` property only because each sub-client has a set of linked domestic and international Virtual Account Numbers to send and receive funds.

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  updateSubClient(
     id: "660fef8e1f3b5452bd6945ec"
     input: {
      externalId: "my_system_id_29f-ae0978b00d09e"
    }
  ) {
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

