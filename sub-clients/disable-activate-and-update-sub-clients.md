# Disable, Activate and Update sub-clients

You can disable and activate sub-clients. Deposits sent to a disabled sub-client will no longer be booked against your balance.

#### Disabling a sub-client

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  disableSubClient(id: "606128f24bf29139b2cf74ef") {
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
{% tab title="Query" %}
```graphql
mutation {
  activateSubClient(id: "606128f24bf29139b2cf74ef") {
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
{
  udpateSubClient(
    id: "5ca18312ace1db0af5784826"
    input: {
      externalId: "my_system_id_29f-ae0978b00d09e"
    }
  ) {
    success
    code
    message
    subClient {
      id
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
    "subClients": [
      {
        "id": "5fb314cb9224595df522db61",
        "fullName": "John Smith",
        "clientType": "INDIVIDUAL",
        "status": "ACTIVE",
        "primaryContact": {
          "firstName": "John",
          "lastName": "Smith"
        },
        "address": {
          "country": "AU"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

