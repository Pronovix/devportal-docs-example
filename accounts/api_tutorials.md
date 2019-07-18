### Tutorials
This tutorial describes how to connect an application to the Account Information APIs in the sandbox environment.

**Important:** You must have a PSD2 license to access production accounts.

**Notes:**
- For information on test data see Sandbox.
- For information on the API operations see Technical.

#### Onboarding
For access to the sandbox it is not required to be onboarded. For information on how to get access to ABN AMRO accounts in production see Overview.

Account Information uses OAuth as authorization method to get access to an account. Click here for information.

You can follow the step-by-step tutorial below to learn how to access the APIs in sandbox environment and production alike.

#### 1. Obtain Consent

Request consent by directing our account holder to the following url (through browser or mobile banking app):

##### Sample request:

```URL
https://auth-sandbox.connect.abnamro.com/as/authorization.oauth2?scope=psd2:account:balance:read+psd2:account:transaction:read+psd2:account:details:read&client_id=TPP_test&response_type=code&flow=code&redirect_uri=https://localhost/auth&bank=NLAA01&state=SilverAdministration-123
```

This will start the consent application. In the consent process, the account holder can select for which account they want to give consent. 

##### Request Attributes

| Attributes | Description |
| --------- | ----------- |
| scope | Indicates for which scope consent is requested. This can be more than one scope. For more information see [Technical](technical#access-token) |
| client_id | In the sandbox environment, use TPP_test as client_id. In production you will receive a client_id from ABN AMRO |
| redirect_uri | In production, it needs to be identical to url that was administered. In sandbox, you must use https://localhost/auth | 
| bank | Use this to select the bank where the account is held. When omitted reverts to NLAA01. See the table below for possible values |
| state | Value that is returned to the calling party. This is used for session management. For example, this could be a reference number, informing you that a certain account holder completed consent |

<br/>

| Bank  | Description |
| --------------------- | ----------- |
| NLAA01 | Consent for an ABN AMRO account in NL or commercial ABN AMRO account in BE, GB or DE |
| BEPB01 | Consent for an ABN AMRO Belgium Private Banking accounts |
| BEPB02 | Consent for an ABN AMRO Belgium Independent Asset Manager accounts |

For more information, see OAuth.

In the response, you will get an OAuth code, which needs to be exchanged within 60 seconds for an Access token and refresh token in step 2.

##### Sample response

```URL
https://localhost/auth?code=9C6UrsGZ0Z3XJymRAOAgl7hKPLlWKUo9GBfMQQEs&state=SilverAdministration-123
```

#### 2. Exchange oauth code

The authorization code needs to be exchanged for an access token and a refresh token. Below is a CURL example:

##### Sample request

```shell
curl -X POST -k https://auth-sandbox.connect.abnamro.com:8443/as/token.oauth2 \
-v \
--cert TPPCertificate.crt \
--key TPPprivateKey.key \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=authorization_code&client_id=TPP_test&code=9C6UrsGZ0Z3XJymRAOAgl7hKPLlWKUo9GBfMQQEs&redirect_uri=https://localhost/auth'
```

##### Request attributes

| Attribute | Description |
| --------- | ----------- |
| grant_type | Indicates which type of authorization is used, it must contain 'authorization_code'  |
| code | The authorizationcode from step 1 |
| redirect_uri | Is mandatory if a redirect_uri was used when requesting consent |

-k is used in sandbox to overrule certificate check because a self-signed certificate is used.

For more details you can refer to the OAuth.

##### Sample Response
```json
{
  "access_token": "{GPgYglX4sO1WhzfChx4tmjr4y7Qg}",
  "refresh_token": "{UHjIAzBZfLGh4dLm8cvEcH6d8BrOmCZXumOpznQBP1}",
  "token_type": "Bearer",
  "expires_in": 7193
}
```

 **Note:** Store the above refresh token for later use. The access token is valid for 2 hours.

#### 3. Check Access using Consent Info

Now you can check for which account and scopes you received consent. You can check the scopes and account for this access token by calling the consent info API.

##### Sample Request

```shell
curl -X GET -k https://api-sandbox.abnamro.com/v1/consentinfo \
-H 'Accept: application/json' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg' \
-H 'Authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg'
```

##### Request attributes

| Attribute | Description |
| --------- | ----------- |
| authorization | The access token from step 2 for which you want to check the consent info |

For more details you can refer to the OAuth.

##### Sample Response
```json
{
  "iban": "NL12ABNA9999876523",
  "transactionId": null,
  "scopes": "psd2:account:details:read psd2:account:balance:read",
  "valid": 1525691979
}'
```

**Note:** You can store the IBAN and scopes for your own adminstration and for access to Account Information APIs.

#### 4. Calling the Account Information APIs {#calling-the-account-information-api}

#### Get details
This operation allows you to get account details such as account holder name.

##### Sample Request

```shell
curl -X GET -k https://api-sandbox.abnamro.com/v1/accounts/NL12ABNA9999876523/details \
-H 'Authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg'
```
##### Request attributes

| Attribute | Description |
| --------- | ----------- |
| authorization | The access token from step 2 for which you want to check the consent info |
| API-Key       | The API Key for your sandbox application. This is obtained from the developer portal |
| accountNumber | The IBAN from step 3 for which the details are to be retrieved |

##### Sample Response
```json
{
  "accountNumber": "NL12ABNA9999876523",
  "currency": "USD",
  "accountHolderName": "E.P.G. Doe"
}
```

#### GET balances

This operation allows you to get the balance for the given accountnumber.

##### Sample Request

```shell
curl -X GET -k https://api-sandbox.abnamro.com/v1/accounts/NL12ABNA9999876523/balances \
-H 'Authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg'
```

##### Sample Response
```json
{
  "accountNumber": "NL12ABNA9999876523",
  "balanceType": "BOOKBALANCE",
  "amount": -3.02,
  "currency": "USD"
}
```

#### GET transactions
This operation fetches the available transaction details for an account.

##### Sample Request
```shell
curl -X GET "https://api-sandbox.abnamro.com/v1/accounts/NL12ABNA9999876523/transactions?bookDateFrom=2019-02-22&bookDateTo=2019-02-28" \
-H 'Authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg'
```

##### Request attributes
| Attribute  | Description  |
| ----- | ------------ |
| bookDateFrom  | Filter, only retrieve transactions more recent than this date (Format: yyyy-mm-dd). If field or date is ommitted, last 50 transactions are retrieved. |
| bookDateTo | Filter, only retrieve transactions preceding this date (Format: yyyy-mm-dd). When used also BookdateTo is conditional. |
| nextPageKey | Reference key of the last retrieved transaction that can be used as query parameter in the next call to fetch the next set of transactions. This key is only provided if more than 50 transactions are present. |

##### Sample Response
```json
{
  "transactions": [
    {

      "mutationCode": "333",
      "descriptionLines": [
        "Payment from Greece",
        "for grapes"
      ],
      "bookDate": "2018-10-23",
      "balanceAfterMutation": 4985,
      "counterPartyAccountNumber": "GR000230201201",
      "counterPartyName": "Greasy Grapes Inc",
      "amount": -15,
      "currency": "EUR",
      "transactionId": "TRXID001"
    },
    {
      "mutationCode": "432",
      "descriptionLines": [
        "Initial cash deposit",
        "made at Shanghai branch",
        "employee: Nick. L."
      ],
      "bookDate": "2018-10-22",
      "balanceAfterMutation": 5000,
      "counterPartyAccountNumber": "",
      "counterPartyName": "",
      "amount": 5000,
      "currency": "EUR",
      "transactionId": "TRXIDC000"
    }
  ],
  "accountNumber": "NL12ABNA9999876523",
  "nextPageKey": "2018-10-24T11:50:27.810000"
}
```

#### GET transactions using next page key
The nextPageKey of the previous response can be used as input.

##### Sample Request
```shell
curl -X GET -k https://api-sandbox.abnamro.com/v1/{accountNumberRequested}/transactions?nextPageKey=2018-10-24T11:50:27.810000 \
-H 'Authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg'
```

#### GET funds
This operation allows you to verify if the amount specified in the request is available on the account. 

##### Sample Request
```shell
curl -X GET -k "https://api-sandbox.abnamro.com/v1/accounts/{NL12ABNA9999876523}/funds?amount=123.45&currency=EUR" \
-H 'Authorization: Bearer GPgYglX4sO1WhzfChx4tmjr4y7Qg' \
-H 'API-Key: X1QTWZre0fnW72l263yrhAWB2FDwx3tg'
```

##### Sample Response
```json
{
  "accountNumber": "NL12ABNA9999876523",
  "amount": 123.45,
  "currency": "USD",
  "available": false
}
```
#### 5. Refresh Access Token
When your access token expires, you can request a new access token by using the refresh token.  

##### Sample Request

```shell
curl -X POST -k https://auth-sandbox.connect.abnamro.com:8443/as/token.oauth2 \
-v \
--cert TPPCertificate.crt \
--key TPPprivateKey.key \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=refresh_token&client_id=TPP_test&refresh_token=UHjIAzBZfLGh4dLm8cvEcH6d8BrOmCZXumOpznQBP1&scope=psd2:account:balance:read&psd2:account:transaction:read'
```

##### Sample Response
```json
{
  "access_token": "{your_new_access_token}",
  "refresh_token": "{your_new_refresh_token}",
  "token_type": "Bearer",
  "expires_in": 7193
}
```

