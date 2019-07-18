### Sandbox
The sandbox and production environments are functionally identical, the technical and authorization documentation applies to both environments. However, the sandbox is static, which means that you can perform all operations that are known in production without making any transactions on an account. The transactions posted in the sandbox are cleaned everyday. 

#### Privacy {#privacy}
It is not allowed to do tests in our sandbox with transactions that contain sensitive/private information.
All account information is production like, but have been fully anonymized and account numbers are fictive.

#### Access {#access}

> Sandbox url: https://api-sandbox.abnamro.com

Use the following credentials for the sandbox:  
| Attribute | Value for Sandbox |
| --------- | ----------------- |
| client_id | TPP_test |
| API-Key | The [API Key](/user/me/apps) for your app from the Developer Portal |
| redirect_uri | https://localhost/auth |

<br/>

| Certificate File |
| ---------------- |
| **Download :** [Public certificate file](/sites/default/files/TPPcertificate.crt) |
| **Download :** [Private key file](/sites/default/files/TPPprivateKey.key) |
  
Depending on your setup you may need to create an exception for the selfsigned certificate in your client.  

For more information on how to access the sandbox environment, see [Technical](technical) and [Tutorials](tutorials). 
Authorization is handled in the same way for production and sandbox environments. For more information see [Authorization](authorization).

#### Accounts {#accounts}
The following accounts are available for testing:

| Account type | IBAN | Description |
| ------------ | ---- | ----------- |
| Corporate Account  | NL62ABNA9999841479 | Many transactions for most bookdays dating back 24 days. Use this to test Next Page key |
| Commercial Account | NL12ABNA9999876523 | Transactions for most bookdays dating back 24 days |
| Retail Account     | NL58ABNA9999142181 | Some Transactions dating back 24 days | 

#### Account Holder Consent {#account-holder-consent}
A simplified version of the consent application can be used to select one of the pre-configured accounts for authorization. This application lacks production security for user e.dentifier authorization in favor of easier development.

#### Error responses {#error-responses}
Returned errors are production-like. However, only functional error scenarios are handled in the sandbox.

#### Requests and Responses {#request-response}
When developing code, please keep in mind that new attributes and values may be added to existing API requests and responses. If a change is deemed code breaking, we will add a new version to both Sandbox and Production.

#### Tutorials
For more information on how to use the sandbox, see [Tutorials](tutorials).
