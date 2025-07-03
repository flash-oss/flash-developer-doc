# Create sub-clients

There are two types of sub-clients: `company` and `individual`.&#x20;

For every `company` registered as a sub-client, there must be one contact person for individual data submitted. Ideally, the contact person should be a **company director** or have a similar role. Therefore, if you are creating a sub-client of the `company` type, we require you to provide **extra** **details**:

* `legalName` - company legal name
* `businessNumber` - company business number (e.g. ABN in Australia)

If the above fields are not set, the sub-client will be created as `individual` type.

{% hint style="warning" %}
This action creates a real account number. If you ever submit fake, unreal, testing, or incorrect data - you will be immediately **blocked** from Flash Payments services.

Please add all possible precautions, processes, staff training, warning messages, and validation checks to your system(s) before creating a sub-client.

Please follow our latest requirements for the proper sub-client data submission:

1. Provide proper`firstName`and`lastName`
2. Provide proper`mobile`number
3. Provide proper `dob` : **the person must be under 65 years of age**&#x20;
4. Provide proper residential `address` Including unit and street number. The sub-client address should correspond to your approved use case from the contract. By default, you're only allowed to have local Australian sub-accounts. **Any non-Australian entities will undergo extended due diligence based on their location and industry relevance for Flash Payments.**  &#x20;
5. Provide proper`idDoc` (`type`, `docNumber`, `issuer` (optional), `issueDate` (optional), `expiryDate` (optional), and `country` ) based on the sub-client contact person's address. For Australian residents, either a driver’s license or a passport is accepted. For non-Australian residents, only a passport is accepted as a document type.
6. Sometimes, we ask our partners to provide “instructing institution” information, but only if you are creating this VAN on behalf of another financial institution. More about institutions [here](../institutions.md). You may provide the ID of the already created institution via the field `instructingInstitutionId` or as a full object via the `instructingInstitution` field.
{% endhint %}

The above personal data submission requirements should be as equally followed for the company contact person, with the exception of `address` property, which can be a company address in this case. &#x20;

To create a sub-client, you need to execute the `createSubClient` mutation as below. You can find the description of each field in the GraphQL API schema.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const bodyJSON = {
  variables: { 
    input: {
      legalName: "Chineese Tradings", 
      businessNumber: "330782000329701", 
      firstName: "John", 
      lastName: "Smith", 
      email: "john.smith@example.com",
      mobile: "+61422832849",
      dob: "1979-05-12",
      address: {
        building: "25",
        street: "Xihu Road, Yuexiu District",
        suburb: "Guangzhou City",
        state: "Guangdong Province",
        postcode: "510030",
        country: "CN",
      },
      idDoc: {
        type: "passport",
        docNumber: "FF1948394",
        issuer: "Australian Passport Office (APO)",
        issueDate: "2000-01-01",
        expiryDate: "2045-01-01",
        country: "AU",
      },
      externalId: "991188227733",
    },
  },
  query: `
mutation ($input: CreateSubClientInput!) {
  createSubClient(input: $input) {
    success code message
    subClient {
      id legalName businessNumber fullName clientType status
      primaryContact {
        firstName lastName email mobile dob
      }
      address {
        country
      }      
      bsb accountNo externalId
    }
  }
}`,
};
```
{% endtab %}

{% tab title="GraphQL Query" %}
```graphql
mutation($input: CreateSubClientInput!) {
  createSubClient(input: $input) {
    success
    code
    message
    subClient {
      id
      legalName
      businessNumber
      fullName
      clientType
      status
      primaryContact {
        firstName
        lastName
        email
        mobile
        dob
      }
      address {
        country
      }
      bsb
      accountNo
      externalId
      # more properties available, see API schema
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```javascript
{ 
  "input": {
    "legalName": "Chineese Tradings",
    "businessNumber": "330782000329701",
    "firstName": "John",
    "lastName": "Smith",
    "email": "john.smith@example.com",
    "mobile": "+61422832849",
    "dob": "1979-05-12",
    "address": {
      "building": "25",
      "street": "Xihu Road, Yuexiu District",
      "suburb": "Guangzhou City",
      "state": "Guangdong Province",
      "postcode": "510030",
      "country": "CN"
    },
    "idDoc": {
      "type": "passport",
      "docNumber": "FF1948394",
      "issuer": "Australian Passport Office (APO)",
      "issueDate": "2000-01-01",
      "expiryDate": "2045-01-01",
      "country": "AU"     
    },
    "externalId": "991188227733"
  }
}
```
{% endtab %}

{% tab title="Response" %}
```javascript
{
  "data": {
    "createSubClient": {
      "success": true,
      "code": "SUBCLIENT_CREATED",
      "message": "Sub-client was successfully created",
      "subClient": {
        "id": "606d28675a2d931bc925fec2",
        "legalName": "Chineese Tradings",
        "businessNumber": "330782000329701",
        "fullName": "John Smith",
        "clientType": "INDIVIDUAL",
        "status": "ACTIVE",
        "primaryContact": {
          "firstName": "John",
          "lastName": "Smith",
          "email": "john.smith@example.com",
          "mobile": "+61 422 832 849",
          "dob": "1979-05-12"
        },
        "address": {
          "country": "CN"
        },
        "bsb": "802919",
        "accountNo": "1066419",
        "externalId": "991188227733"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
