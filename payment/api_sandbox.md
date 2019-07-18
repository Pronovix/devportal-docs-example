### Sandbox
The sandbox enviroment is functionally identical to production, the technical and authorization documentation applies to both environments.

> The happy flow is fully production like. However not all rainy scenario's are covered.

The sandbox is static. That means that you can do all operations known in production, but this will not result in real transactions happening on the account. Also the sandbox is recycled every day, so you start with a clean sandbox.

#### Privacy {#privacy}
It is not allowed to do tests in our sandbox with transactions that contain sensitive/private information.
All account numbers are fictive. All account information is production like, but have been fully anonymized.

#### Access {#access}
The URL for the sandbox environment can be found in [Technical](technical). The OAUTH for sandbox is the same as for production. Please check [Authorization](authorization) for details.

You can use following credentials for the sandbox:  
| Attribute | Value for Sandbox |
| --------- | ----------------- |
| client_id | TPP_test |
| API-Key | The Consumer Key for the sandbox for your app from the Developer Portal |
| redirect_uri | https://localhost/auth |

<br/> 

| Certificate File |
| ---------------- |
| **Download :** [Public certificate file](/sites/default/files/TPPcertificate.crt) |
| **Download :** [Private key file](/sites/default/files/TPPprivateKey.key) |
  
You may need to create an exception for the selfsigned certificate in your client.  
   
#### Accounts {#accounts}
The following accounts are available for testing.

| Account type | IBAN |
| ------------ | ---- |
| Corporate Account  | NL62ABNA9999841479 |
| Commercial Account | NL12ABNA9999876523 |
| Retail Account     | NL58ABNA9999142181 |

All account numbers are fictive and modulus 97 proof.

#### Account Holder Consent {#account-holder-consent}
A simplified version of the consent application can be used to select one of the pre-configured accounts for authorization. This application is lacking production security for user authorization in favor of usability for development.

#### Error responses {#error-responses}
The returned errors are productionlike. However only functional error scenario's are covered in the sandbox.

#### Requests and Responses {#request-response}
New attributes and values may be added to existing API requests and responses. Please take this into account when developing code. A change that is deemed code breaking will result in a new version both on Sandbox and Production.

#### Tutorial
Please check our [Tutorial](tutorials) how to consume the sandbox.

