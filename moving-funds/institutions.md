---
description: CRUD queries for Institutions
---

# Institutions

### Instructing Institution

`Institution` entity is a piece of information about the party who instructed you to make the withdrawal. This data is important for compliance within Australia. This is not the sender. If there is no other institution who has instructed this withdrawal, leave this blank and your own details will be used for compliance and auditing purposes.&#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Institution explanation</p></figcaption></figure>

#### Creating institutions

* via Flash Connect (you can copy an ID of an existing Institution).&#x20;
* via API by invoking `createInstitution` mutation.&#x20;
* by providing `instructingInstitution` object into `createWithdrawal` mutation.&#x20;

{% hint style="warning" %}
Please avoid creating multiple Institutions for the same organisation because they are used for compliance reporting and subject to review.&#x20;
{% endhint %}

{% hint style="info" %}
Before creating or updating an institution we will try to find an existing one by:&#x20;

* By `instructingInstitution.externalId`
* By `instructingInstitution.businessNumber` AND `instructingInstitution.address.country`
* By `instructingInstitution.legalName` AND `instructingInstitution.address.country`
{% endhint %}



### Creating institution example

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: {
    input: {
      legalName: "Intermediate Institution Ltd",
      businessNumber: "A39477669937",
      address: {
        postcode: "2000",
        street: "203 Business Street",
        country: "AU",
        state: "NSW",
        suburb: "Sydney",
    },
  },
},
query: `
mutation ($input: InstitutionInput!) {
  createInstitution(input: $input) {
    success code message    
    institution {     
      id    
    }  
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: InstitutionInput!) {
  createInstitution(input: $input) {
    success
    code
    message
    institution {
      id
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
 "input": {
    "legalName": "Intermediate Institution Ltd",
    "businessNumber": "A39477669937",
    "address": {
      "postcode": "2000",
      "street": "203 Business Street",
      "country": "AU",
      "state": "NSW",
      "suburb": "Sydney"
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createInstitution": {
      "success": true,
      "code": "INSTITUTION_CREATED",
      "message": "Institution created",
      "institution": {
        "id": "65570da4f176682c5e412552"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
