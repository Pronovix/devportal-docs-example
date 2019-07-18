### Tutorials

On this page you will find a full tutorial, which will help you to connect the application you are developing to the Payment Initation API's in the sandbox environment.

**Note:**

- Check the section on Sandbox, where you can find test data and flows.
- For detailed information on the API operations, please refer to the Technical.
- For detailed information on the authorization & authentication, please refer to the OAuth and the Authorization page.

#### Onboarding {#onboarding}

Please check Overview on how you can get access to ABN AMRO Payment Initiation API's.

**Note:** Before starting development, please note you need a PSD2 license to get access to accounts in production.

Payment Initiation uses OAuth as authorization method to get access to an account. Click here for more information on how to authenticate.

Once onboarding has been completed, you can follow the step-by-step tutorial below to learn how to access the API's in sandbox environment.

#### 1. Access Token for Payment Registration {#access}

First you need to get an access token so you can register the payment:

##### Sample Request SEPA or Crossborder Payment

```shell
curl -X POST -k https://auth-sandbox.connect.abnamro.com:8443/as/token.oauth2 \
-v \
--cert TPPCertificate.crt \
--key TPPprivateKey.key \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=client_credentials&client_id=TPP_test&scope=psd2:payment:sepa:write psd2:payment:xborder:write'
```

##### Request Attributes

| Attribute | Description |
| --------- | ----------- |
| grant_type | The indicator of the type of authorization flow being used, it should contain 'client_credentials' |
| scope | The scope parameter is used for which scopes access is needed. This can be more than one scope. [See Technical](technical#access-token). |
| client_id | In the sandbox environment you can use TPP_test as client_id. In production you will receive a client_id from ABN AMRO |

-k is used in sandbox to overrule certificate check because a self-signed certificate is used.

For more details you can refer to the OAuth page.

##### Sample Response

```json
{
  "token_type": "Bearer",
  "expires_in": 7199,
  "access_token": "X1PTWZre0fnW72l263yrhAWB2FDwx3tg"
}
```

#### 2. Register Payment {#register-payment}

Now you can register the payment which the account holder can authorize in the next step. You can register a SEPA payment (payment in EUR and inside Monetarian Union) or a crossborder payment (Non EUR payment or EUR payment outside EMU).

##### Sample Request SEPA Payment

```shell
curl -X POST -k https://api-sandbox.abnamro.com/v1/payments  \
-v \
-H 'accept: application/json'  \
-H 'authorization: Bearer X1PTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'content-type: application/json'  \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-d '{
"initiatingpartyAccountNumber": "NL62ABNA9999841479",
"counterpartyAccountNumber": "NL12ABNA9999876523",
"amount": 149.99,
"counterpartyName": "John Doe",
"remittanceInfo": "Payment of invoice 123/01"
  }'
```

##### Sample Request structured and future dated SEPA Payment

```shell
curl -X POST -k https://api-sandbox.abnamro.com/v1/payments  \
-v \
-H 'accept: application/json'  \
-H 'authorization: Bearer UTUZnSKhYEYhX9qWl03epLVC3jyD' \
-H 'content-type: application/json'  \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-d '{
"initiatingpartyAccountNumber": "NL62ABNA9999841479",
"counterpartyAccountNumber": "NL12ABNA9999876523",
"amount": 149.99,
"requestedExecutionDate": "2019-01-30",
"counterpartyName": "John Doe",
"structuredRemittanceInfo": {
    "issuer": "CUR",
    "reference": "12345"
    }
  }'
```

##### Request Attributes

| Attribute | Description |
| --------- | ----------- |
| authorization | The access token from the previous which allows you to register the payment |
| API-Key       | Consumer key from the developer portal for your sandbox app |
| initiatingpartyAccountNumber | The account number in IBAN format of the party initiating the transaction. If omitted the accountnumber will be selected during the authorization of the payment. Also the accountnumber can be changed by the accountholder during the authorization of the payment |
| requestedExecutionDate       | The date the payment needs to be executed. If ommitted the payment will be directly processed |
| structuredRemittanceInfo     | A group identyifing structured remittance information. Either unstructured remittanceInfo can be used or structuredRemittanceInfo |
| issuer                       | 3 types of issuers for structured remittance information are supported. CUR for Dutch payment reference, BBA for Belgium payment reference and ISO for ISO payment reference | 
| reference                    | The structured remittance information |

Only the attributes that require some additional explanation are mentioned. You can check the other attributes in Technical. 

##### Sample Response
```json
{
  "accountNumber": "NL62ABNA9999841479",
  "transactionId": "321463282363179XX",
  "status": "STORED"
}'
```

You should store the transactionId since it is later needed for checking the authorization by the accountholder and for executing the authorized payment.

##### Sample Request Xborder Payment

```shell
curl -X POST -k https://api-sandbox.abnamro.com/v1/payments/xborder \
-v \
-H 'accept: application/json'  \
-H 'authorization: Bearer X1PTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'content-type: application/json'  \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-d '{
  "initiatingParty": {
	"accountNumber": "NL62ABNA9999841479",
	"accountCurrency": ""
  },
  "counterParty": {
  "name": "John Doe",
  "accountNumberType": "IBAN",
  "accountNumber": "NL12ABNA9999876523",
  "bankIdentifierType": "SWIFTBIC",
  "bankIdentifier": "ABNANL2A"
  },
  "amount": 3.78,
  "currency": "USD",
  "chargesBearer": "SHA",
  "remittanceInfo": "A text with details of the payment"
  }'
```

##### Request Attributes

| Name  | Description  |
| ----- | ------------ |
| Authorization               | Access token from the previous step |
| API-Key                     | Consumer key from the developer portal for your sandbox app |
| accountNumber | The accountnumber in IBAN format of the party initiating the transaction. The accountnumber can be changed by the accountholder during the authorization of the payment |

##### Sample Response

```json
{
  "accountNumber": "NL62ABNA9999841479",
  "transactionId": "321463282363179XX",
  "status": "STORED"
}'
```

**Note:** You should store the transactionId since it is later needed for checking the authorization by the accountholder and for executing the authorized payment.

#### 3. Request Consent {#request-consent}

You can request consent by directing our account holder to the following url:

##### Sample Request for Consent SEPA Payment

```
https://auth-sandbox.connect.abnamro.com/as/authorization.oauth2?scope=psd2:payment:sepa:write+psd2:payment:sepa:read&client_id=TPP_test&transactionId=123&response_type=code&flow=code&redirect_uri=https://localhost/auth&bank=NLAA01&state=Paymentreference123
```
##### Sample request for consent xborder payment

```
https://auth-sandbox.connect.abnamro.com/as/authorization.oauth2?scope=psd2:payment:xborder:write&client_id=TPP_test&transactionId=123&response_type=code&flow=code&redirect_uri=https://localhost/auth&bank=NLAA01&state=Paymentreference123
```

Any of these examples will start the consent application. In the consent flow the account holder can select for which account they want to give consent. see sandbox for details.

##### Request Attributes

| Attribute | Description |
| --------- | ----------- |
| scope | The scope attribute is used for which scopes access is needed. This can be more than one scope. See Technical Use the value psd2:payment:sepa:write for releasing a registered SEPA payment and psd2:payment:sepa:read for checking status |
| client_id | In the sandbox environment you can use TPP_test as client_id. The sandbox will be available in February/March. In production you will receive a client_id from ABN AMRO |
| transactionId | Unique id that was generated during the registration of a payment |
| redirect_uri | In production, it needs to be identical to url that was administered. In sandbox, you can choose any url, including local host. | 
| bank | This is used to select the bank where the account is held for which consent is being requested. When omitted reverts to NLAA01. See the table below for possible values |
| state | This attribute is returned to the calling party which can be used for session management. You could e.g. indicate your reference number here, so you will know which transaction you will receive consent |

| Bank  | Description |
| --------------------- | ----------- |
| NLAA01 | Consent for ABNAMRO account in NL or commercial ABN AMRO account in BE, GB or DE |
| BEPB01 | Consent for ABN AMRO Belgium Private Banking accounts |
| BEPB02 | Consent for ABN AMRO Belgium Independent Asset Manager accounts |

For more details you can refer to the OAuth page.

In the response, you will get an OAuth code, which needs to be exchanged within 10 seconds for an Access token and refresh token in [step 4](#exchange-access-token).

##### Sample response

```URL
https://localhost/auth?code=9C6UrsGZ0Z3XJymRAOAgl7hKPLlWKUo9GBfMQQEs&state=Paymentreference123
```

**Note:**

- The access code is valid for only 10 seconds. You need to exchange the access code quickly.
- When consent is cancelled / not successful, an error is returned (error_description=Unexpected+Runtime+Authn+Adapter+Integration+Problem.&error=server_error#). The registered payment is also cancelled in that case, and cannot be authorized anymore.

#### 4. Exchange Access Code Token {#exchange-access-token}

Next the authorization code needs to be exchanged for an access token and a refresh token. Below is a CURL example:

##### Sample Request

```shell
curl -X POST -k https://auth-sandbox.connect.abnamro.com:8443/as/token.oauth2 \
-v \
--cert TPPCertificate.crt \
--key TPPprivateKey.key \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=authorization_code&client_id=TPP_test&code=9C6UrsGZ0Z3XJymRAOAgl7hKPLlWKUo9GBfMQQEs&redirect_uri=https://localhost/auth'
```

##### Request Attributes

| Attribute | Description |
| --------- | ----------- |
| grant_type | The indicator of the type of flow being used, it should contain 'authorization_code' |
| code | The authorization code from the previous step |
| redirect_uri | Is mandatory When redirect_uri was used for requesting consent |

For more details, refer to the OAuth.

##### Sample Response 

```json
{
  "access_token": "GPgYglX4sO1WhzfChx4tmjr4y7Qg",
  "refresh_token": "UHjIAzBZfLGh4dLm8cvEcH6d8BrOmCZXumOpznQBP1",
  "token_type": "Bearer",
  "valid": 7193
}
```

**Note:** Store refresh token for later use (the access token is valid for 2 hours) 

#### 5. Check Authorization using Consent Info {#check-access-using-consent-info}

For the next step, you need an access token and transactionId to execute the authorized payment. There are two ways to find the transactionId:

1. Using state parameter in consent flow. When starting the consent flow you can link the state reference number to the transactionId. When state parameter is used Consent Info is not needed in this use case.

2. Using Consent Info you can check the transactionId (also scopes and account number) for this access token by calling the consent info API.

##### Sample Request

```shell
curl -X GET -k https://api-sandbox.abnamro.com/v1/consentinfo \
-v \
-H 'accept: application/json' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg'
```

##### Request Attributes

Please see [OAuth](/get-started#authentication) for details. Only parameters that need clarification are mentioned here.

| Attribute | Explanation |
| --------- | ----------- |
| authorization | The access token from step 4 for which you want to check the consent info |

##### Sample Response
```json
{
  "scopes": "payment:sepa:write payment:sepa:read",
  "iban": "NL62ABNA9999841479",
  "paymentReference": "8338L5812304793S0PD",
  "valid": "1543931986"
}
```

You can store the transactionId (paymentReference) and payment scopes to execute the payment in the next step and to check status. The initiating account number (IBAN) can be used to link the tranaction to the account of the client (during consent he can change pre-selected initiating account).

#### 6. Execute Payment {#execute-payment}

Next you can execute the stored payment that was authorized by the account holder in the previous steps.

##### Sample Request for SEPA Payment

```shell
curl -X PUT -k https://api-sandbox.abnamro.com/v1/payments/8338L5812304793S0PD \
-v \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'accept: application/json' \
-H 'authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg'
```

##### Sample Request for Xborder Payment

```shell
curl -X PUT -k https://api-sandbox.abnamro.com/v1/payments/xborder/8338L5812304793S0PD \
-v \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'accept: application/json' \
-H 'authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg'
```

##### Sample Response

```json
{
  "accountNumber": "NL62ABNA9999841479",
  "transactionId": "8338L5812304793S0PD",
  "status": "EXECUTED"
}
```

If you used the state attribute with consent [(in step 3)](#request-consent), you can determine the initiating account number in the response here. For more information you can refer to the Technical.

#### 7. Check Payment Status {#check-payment-status}

If the status in the response for executing payment is not "EXECUTED" or "REJECTED" you can check at a later time what the status is using following sample request:

##### Sample Request for SEPA Payment

```shell
curl -X GET -k https://api-sandbox.abnamro.com/v1/payments/8338L5812304793S0PD \
-v \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'accept: application/json'  \
-H 'authorization: Bearer {your_access_token}'
```

##### Sample Response SEPA

```json
{
  "accountNumber": "NL62ABNA9999841479",
  "transactionId": "8338L5812304793S0PD",
  "status": "AUTHORIZED"
}
```

#### 8. Refresh Access Token {#refresh-access-token}

When your access token expires you can request a new access token using the refresh token.  

##### Sample Request

```shell
curl -X POST -k https://auth-sandbox.connect.abnamro.com:8443/as/token.oauth2 \
-v \
--cert {location_of_your_certificate} \
--key {location_of_your_private_key} \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=refresh_token&client_id=TPP_test&refresh_token=UHjIAzBZfLGh4dLm8cvEcH6d8BrOmCZXumOpznQBP1&scope=psd2:payment:sepa:write+psd2:payment:sepa:read'
```

##### Sample Response

```json
{
  "access_token": "{mkwAngBIJtlL9TxxNhECHV4LaBBt}",
  "refresh_token": "{nLlBcohGqcAvs2iyQ4SAdenC5moqRh9y3NifBR3j04}",
  "token_type": "Bearer",
  "valid": 7193
}
```
