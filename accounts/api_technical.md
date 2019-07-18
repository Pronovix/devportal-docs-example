### Technical
Account Information are APIs that can used to retrieve account data like transaction information, the current balance or details of the account itself. These APIs can be part of multiple product offerings. Also, the functionality of the API depends on the product offering.

#### Open API Specification
To download the Open API Specification of the Payment Initiation API in YAML format, click here.

#### Payment Initation version 1.0.2
New in this version:
- Updated YAML file to OpenAPI 3 specifications
- Textual improvements based on feedback

#### Environments
- Sandbox: https://api-sandbox.abnamro.com

- Production: https://api.abnamro.com

#### Access token
Account Information API uses client authentication and user-based consent OAuth as described here. To use the API, you need to obtain an access token from the Authentication API with the one or more of the following scopes:

| Operation  | Scope  |  Description |
| ----- | ----- | ----- |
| Details | psd2:account:details:read | To get details of the account. Get this scope through consent from the account holder |
| Balances | psd2:account:balance:read | To read the balance of the account. Get this scope through consent from the account holder |
| Transactions | psd2:account:transaction:read | To get transactions of the account. Get this scope through consent from the account holder |
| Funds | psd2:account:funds:read | To request if sufficient funds are available for amount. Get this scope through consent from the account holder |

#### GET details
>GET /v1/accounts/{accountNumber}/details

This operation allows you to retrieve the details of the account, like currency and account holder name.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization | String | Header |true | Access token to be passed as a Bearer token |
| API-Key       | String | Header |true | The API Key for your app in the Developer Portal |
| accountNumber | String | Path |true | The IBAN for which the details are to be retrieved |

##### Response Attributes


| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| accountNumber     | String | Body   | true  | The IBAN of the request for which the details were retrieved |
| currency          | String | Body   | true  | Currency of the account, 3 characters ISO 4217 currency code (e.g. EUR or USD) |
| accountHolderName | String | Body   | true  | Name of the account holder for consumers or trade name for commercial clients|

#### GET balances
>GET /v1/accounts/{accountNumber}/balances

This operation allows you to retrieve the details of the account, like book balance of the account and currency.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization | String | Header   | true | Access token to be passed as a Bearer token |
| API-Key       | String | Header   | true | The API Key for your app in the Developer Portal |
| accountNumber | String | Path | true | The IBAN for which the balance is to be retrieved |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| accountNumber| String | Body   | true | The IBAN for which the balance was retrieved |
| balanceType  | String | Body   | true | Indicator of the type of balance that was retrieved |
| currency     | String | Body   | true | Currency of the account, 3 characters ISO 4217 currency code (e.g. EUR or USD) |
| amount       | Number | Body   | true | Balance of the account which can be negative when there is a debit balance |

#### GET transactions

>GET /v1/accounts/{accountNumber}/transactions

This operation fetches available transaction details for an account. It can provide both realtime bookings for current date and also bookings for past book dates. By default, the response from the API returns transactions that are available on the account in reversed chronological order.

- In case no transactions are available, a blank response is returned.
- In cases where more than 50 transactions are available on the account, a next page key is returned that can be used to retrieve the next 50 transactions.

Filtering for specific bookdates can be done using from and to date. The history for which transactions can be retrieved is 18 months (identical to Internet Banking).

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization | String | Header  | true  | Access token to be passed as a Bearer token |
| API-Key       | String | Header  | true  | The API Key for your app in the Developer Portal |
| accountNumber | String | Path | true  | The IBAN for which the transactions are to be retrieved |
| bookDateFrom  | String | query   | false | Filter, only retrieve transactions more recent than this date (Format: yyyy-mm-dd). If field or date is ommitted, last 50 transactions are retrieved |
| bookDateTo    | String | query   | false | Filter, only retrieve transactions preceding this date (Format: yyyy-mm-dd). |
| nextPageKey   | String | query   | false |Key parameter for fetching the next set of transactions. Value to be used is returned in a response of a previous request to the endpoint when more transactions are present |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| transactions         | Array  | Body         | false | Transaction details |
| mutationCode         | String | transactions | false | Indicator for the type of transaction |
| descriptionLines     | Array  | transactions | false | Unformatted text entered by the user during the transaction. String array of up to nine lines of each 0 to 32 characters |
| bookDate             | String | transactions | false | The book date of the mutation |
| balanceAfterMutation | String | transactions | false | Account bookbalance after the payment transaction. Minus in case of negative balance |
| counterPartyAccountNumber | String | transactions | false | Counter account number. Will be empty if no counter account is found for the transaction (e.g an ATM withdrawal) |
| counterPartyName     | String | transactions | false | Name associated with the counter account number |
| amount               | String | transactions | true  | Transaction amount including minus in case of transactions deducted from the account |
| currency             | String | transactions | true  | Currency of the mutation, 3 characters alphabetic ISO currency code (e.g. EUR or USD) |
| transactionId        | String | transactions | true  | Unique Transaction id |
| accountNumber        | String | Body         | true  | IBAN of the request for which the transactions were retrieved |
| nextPageKey          | String | Body         | false | Reference key of the last retrieved transaction that can be used as query parameter in the next call to fetch the next set of transactions. This key is only provided if more than 50 transactions are present |

#### GET funds

>GET /v1/accounts/{accountNumber}/funds

This operation will allow you to verify if the amount specified in the request is available on the account at that time (including any creditline).

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization | String | Header   | true  | Access token to be passed as a Bearer token |
| API-Key       | String | Header   | true  | The API Key for your app in the Developer Portal |
| accountNumber | String | Resource | true  | The IBAN for which the details are to be retrieved |
| amount        | Number | Query    | true  | Amount, decimal always a positive number and no more than 3 decimals (fractional digits) |
| currency      | String | Query    | false | Currency of the amount, 3 characters alphabetic ISO-4217 currency code (e.g. EUR or USD). If no currency is provided, EUR is assumed |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| accountNumber | String  | Body | true | IBAN of the request for which the details were retrieved |
| amount        | Number  | Body | true | Amount as specified in the request, decimal always a positive number and no more than 3 decimals (fractional digits) |
| currency      | String  | Body | true | Currency of the amount, 3 characters alphabetic ISO currency code (e.g. EUR or USD) as per ISO-4217 |
| available     | Boolean | Body | true | Boolean indication if the requested amount is available |

####  Check Access using Consent Info
>GET /v1/consentinfo

This operation will provide information regarding the authorization to a resource that an access token provides. The access token "reprents" the consent granted by the account holder or resource owner to an account or resource. The information returned contains information about the granted scopes, selected account number or transaction id.

The usecase of Consent Info is depicted in the following sequence diagram.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization | String | Header| true | Access token to be passed as a Bearer token. |
| API-Key       | String | Header| true | The API Key for your app in the Developer Portal |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| iban          | String | Body   | false | The iban/account number associated with the access token |
| transactionId | String | Body   | false | The transaction ID associated with the access token for registered payments. Is "null" when access token is not for payment |
| scopes        | String | Body   | false | The scopes associated with the access token |
| valid         | Number | Body   | true  | Time that the token is valid (in Unix epoch format) |


**Note:** You can store the iban, transactionId and scopes for your own adminstration and for access to the API.

#### Error Response & Codes
This section describes the error response & the codes being sent by the API.

##### Error Response Attributes
| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| code | String | Body   | true  | The code of the error |
| message | String | Body | true | The human readible error message |
| reference | String | Body | true | Reference where to find more information on error |
| Trace-Id | String  | Body | true | Unique id generated for every request |
| status | String | Body | true | https error code, 4xx or 5xx |
| category | String | Body | true | Category of error. Values: BAD_REQUEST, FORBIDDEN, INTERNAL_SERVER_ERROR, BACKEND_ERROR, more generic categories can be found on the Get Started page under the section Error Codes |

##### Error codes
This section lists the errors that are particular for this API. If your error is not listed here, or you want to know which general errors can occur, you can check on the Get Started page under the section Error Codes.

| HTTP status code | Error Code | Error Description |
| ---------------- | --------------- | ---------------------- |
| 400 | MESSAGE_RST560_0001 | Account number empty or null |
| 400 | MESSAGE_RST560_0007 | IBAN is invalid |
| 400 | MESSAGE_RST560_0008 | To date is invalid |
| 400 | MESSAGE_RST560_0009 | From date is invalid |
| 400 | MESSAGE_RST560_0010 | Account number too long |
| 400 | MESSAGE_RST560_0011 | From date is greater than to date |
| 400 | MESSAGE_RST560_0012 | To date is greater than current date |
| 400 | MESSAGE_RST560_0013 | From date is greater than current date |
| 400 | MESSAGE_RST560_0014 | Amount is invalid |
| 400 | MESSAGE_RST560_0015 | Currency is invalid |
| 400 | MESSAGE_RST560_0018 | Amount is blank |
| 400 | MESSAGE_RST560_0022 | Invalid book date |
| 400 | MESSAGE_RST560_0023 | Book date is in future |
| 400 | MESSAGE_BAI560_0029 | From date is more than 18 months in the past |
| 403 | MESSAGE_RST560_0016 | No access to account. Please contact the account holder for details |
| 500 | MESSAGE_BAI560_0005 | Report download service failure. No data exists for the request (e.g. no bookings available yet or not a bookday). Solution is to choose another date or try later |
| 5xx | MESSAGE_xxxxxxxxxxx | For any other 500 error please contact the bank if problem persists |

#### Additional info

- When POST fails you can retry later. Likely it will succeed when the service is available again.
- When reposting, please do not use short retry periods to keep out of rate limiting scenario's.

#### Previous releases

##### v1.01
* Sandbox now live
* Updated OAuth2 in YAML file
* Updated documentation for Sandbox
* Minor improvements
