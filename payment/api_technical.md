### Technical

Payment Initiation API's can be used to send payment orders for a current account and check their processing status. These API's can be part of multiple product offerings. Also, the functionality of the API depends on the product offering.

#### Updated in v1.01
* Sandbox now live
* Updated OAuth2 in YAML file
* Updated documentation for Sandbox
* Minor improvements

#### Open API Specification {#open-api-specification}

Click here to download the Open API Specification of the Payment Initiation API in yaml format.

#### Environments

Sandbox:
https://api-sandbox.abnamro.com

Production:
https://api.abnamro.com

#### Access Token {#access-token}

Payment Initiation API uses client authentication OAuth for registring payments as described here. To use the API, you need to obtain an access token from the Authentication API with the one or more of the following scopes:

##### SEPA Scopes

| Operation | Scopes SEPA | Description |
| --------- | :---------: | ----------- |
| POST Payments | psd2:payment:sepa:write | To register a SEPA payment for authorization by the accountholder. You can get this scope through client credentials |
| PUT Payments | psd2:payment:sepa:write | To release SEPA payment after authorization by the accountholder. You get this scope through consent from the account holder |
| GET Payments | psd2:payment:sepa:read | To request the status of a SEPA payment. You get this scope through consent from the account holder |

##### XBORDER Scopes

| Operation | Scopes non-SEPA or UK | Description |
| --------- | :-------------------: | ----------- |
| POST Payments | psd2:payment:xborder:write | To register a xborder payment for authorization by the accountholder. You can get this scope through client credentials |
| PUT Payments | psd2:payment:xborder:write | To release xborder payment after authorization by the accountholder. You get this scope through consent from the account holder |

#### POST Payments {#post-payments}

>POST v1/payments

This operation will register a SEPA payment at the bank for further authorization by the account holder through the consent application. The access token from the consent can be used in PUT payments to execute the registered payment.

**Note:** UK payments in EUR have to be sent in using payments/xborder.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization                | String | Header                   | true  | Access token to be passed as a Bearer token |
| API-Key                      | String | Header                   | true  | Consumer key obtained after app registration on developer portal |
| initiatingpartyAccountNumber | String | Body                     | false | The account number in IBAN format of the ordering party, initiating the transaction, if omitted the account number will be selected during the authorization of the payment. Also the account number can be changed by the account holder during the authorization of the payment |
| counterpartyAccountNumber    | String | Body                     | true  | The account number in IBAN format of the counterparty |
| counterpartyName             | String | Body                     | true  | The name of the counterparty. 70 characters are allowed |
| amount                       | Number | Body                     | true  | The amount of the transaction always positive |
| currency                     | String | Body                     | false | The currency of the transaction, only EUR allowed. If omitted EUR is assumed |
| requestedExecutionDate       | String | Body                     | false | The date the payment needs to be executed. If ommitted the payment will be directly processed |
| remittanceInfo               | String | Body                     | false | Information for the beneficiary about the payment. 140 characters are allowed |
| structuredRemittanceInfo     | Object | Body                     | false | A group identifying structured remittance information. Either unstructured remittanceInfo can be used or structuredRemittanceInfo |
| issuer                       | String | structuredRemittanceInfo | true  | 3 types of issuers for structured remittance information are supported. CUR for Dutch payment reference, BBA for Belgium payment reference and ISO for ISO payment reference | 
| reference                    | String | structuredRemittanceInfo | true  | The structured remittance information | 

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Trace-Id | String | Header | true | Unique id generated for every request |
| accountNumber | String | Body   | false | IBAN of the initiating party |
| transactionId | String | Body   | true  | Unique id generated for the registered payment. Is required input for PUT Payments |
| status        | String | Body   | false |  The current status of the payment which is STORED after successful registration |

#### PUT Payments {#put-payments}

>PUT /v1/payments/{transactionId}

This operation will execute an authorized and registered SEPA payment at the bank. Authorization by the account holder is needed through the consent application to obtain a access token. The TransactionId is received as response of the POST Payments.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization    | String | Header   | true | Access token to be passed as a Bearer token |
| API-Key          | String | Header   | true | Consumer key obtained after app registration on developer portal |
| transactionId    | String | Path | true | Unique id generated for the registered payment |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Trace-Id      | String | Header | true  | Unique id generated for every request |
| accountNumber | String | Body   | false | IBAN of the initiating party |
| transactionId | String | Body   | true  | Unique id generated for the registered payment |
| status        | String | Body   | false | The current status of the payment which is IN PROGRESS, SCHEDULED, EXECUTED or REJECTED after successful execution (PUT) |


#### GET Payments {#get-payments}

>GET /v1/payments/{transactionId}

This operation will retrieve the current status of a payment.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization    | String | Header   | true | Access token to be passed as a Bearer token |
| API-Key          | String | Header   | true | Consumer key obtained after app registration on developer portal |
| transactionId    | String | Path | true | Unique id generated for the payment |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Trace-Id      | String | Header | true  | Unique id generated for every request |
| accountNumber | String | Body   | false | IBAN of the initiating party |
| transactionId | String | Body   | true  | Unique id generated for the registered payment |
| status        | String | Body   | false | The current status of the payment. See Status Values table below for details |

#### Status Values {#status}

| Value | Description |
| ----- | ----------- |
| STORED | Payment has been sent in using POST, but no consent has been received yet |
| AUTHORIZED | Payment has been authorized through consent by account holder. Payment can be executed using PUT Payments |
| INPROGRESS | Registered payment has been released and is being processed |
| SCHEDULED | Future dated payment is scheduled for processing |
| EXECUTED | Payment is booked |
| REJECTED | Payment is rejected. This can be for example because of invalid beneficiary IBAN, insufficient funds or account restrictions |
| UNKNOWN | Status cannot be retrieved. Try later again |

#### POST payments/xborder {#post-payments-xborder}

>POST /v1/payments/xborder

This operation will register a non-SEPA payment at the bank for further authorization by the account holder through the consent application. The access token from the consent can be used in PUT payments to execute the registered payment. Due to the nature of international interbank communication formats, these type of payments are subject to SWIFT characterset limitations and possible truncation of fields.

**Note:** Non-SEPA payments are payments made outside the European Economic Area and/or non-EURO currency. Also payments in the UK need to be sent in as crossborder.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization               | String | Header          | true  | Access token to be passed as a Bearer token |
| API-Key                     | String | Header          | true  | Consumer key obtained after app registration on developer portal |
| initiatingParty             | Object | Body            | false | A group identifying the account to be used to initiate the payment from, if omitted the accountnumber will be selected during the authorization of the payment |
| accountNumber               | String | initiatingParty | true  | The account number in IBAN format of the ordering party, initiating the transaction, the account number can be changed by the account holder during the authorization of the payment |
| accountCurrency             | String | initiatingParty | false | Currency of the account number, 3 characters ISO 4217 currency code (e.g. EUR or USD). If not provided, the currency of the amount is assumed as the currency of the initiating account |
| counterParty                | Object | Body            | true  | A group identyifing the account of the counter party |
| name                        | String | counterParty    | true  | The name of the counterparty |
| accountNumberType           | String | counterParty    | true  | Indicator of the type of accountNumber being used, either IBAN or BBAN (latter for domestic/basic formatting) |
| accountNumber               | String | counterParty    | true  | The IBAN or BBAN formatted account number of the counterparty as indicated by accountNumberType |
| bankIdentifierType          | String | counterParty    | true  | Indicator signifying the type of bankIdentifier used; SWIFTBIC (for a BIC); UKSORTCODE (for a UK sortcode); FEDWIRE (for a US bankcode) |
| bankIdentifier              | String | counterParty    | true  | Specify the BankIdentifier here for the selected bankIdentifierType |
| amount                      | Number | Body            | true  | The amount of the transaction, always positive |
| currency                    | String | Body            | true  | Currency of the transaction, 3 characters alphabetic ISO-4217 currency (e.g. EUR, USD) |
| requestedExecutionDate      | String | Body            | false | The date the payment needs to be executed. If ommitted the payment will be directly processed (or when clearing is closed at first possible time) | 
| chargesBearer               | String | Body            | false | Indicator who pays the charges related to the payment BEN = beneficiary, OUR = initiating party, SHA both parties share the charges. If not specified, SHA is assumed. Note in EEA always use SHA |
| remittanceInfo              | String | Body            | false | Descriptive text that is part of the transaction. 140 characters are allowed |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Trace-Id      | String | Header | true  | Unique id generated for every request |
| accountNumber | String | Body   | false | IBAN of the initiating party |
| transactionId | String | Body   | true  | Unique id generated for the registered payment |
| status        | String | Body   | false | The current status of the payment which is STORED after successful registration |

#### PUT payments/xborder {#put-payments-xborder}

>PUT /v1/payments/xborder/{transactionId}

This operation will execute an authorized and registered non-SEPA payment at the bank. Authorization by the account holder is needed through the consent application to obtain a access token. The transactionId is received as response of the POST Payments.

##### Request Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Authorization    | String | Header   | true | Access token to be passed as a Bearer token |
| API-Key          | String | Header   | true | Consumer key obtained after app registration on developer portal |
| transactionId    | String | Path | true | Unique id generated for the registered payment |

##### Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| Trace-Id      | String | Header | true  | Unique id generated for every request |
| accountNumber | String | Body   | false | IBAN of the initiating party |
| transactionId | String | Body   | true  | Unique id generated for the registered payment |
| status        | String | Body   | false | The current status of the payment which is IN PROGRESS, SCHEDULED, EXECUTED or REJECTED after successful execution (PUT) |

#### Error Response & Codes {#error-response-code}

This section describes the error response & the codes being sent by the API.

##### Error Response Attributes

| Name  | Type  | In  | Required  | Description  |
| ----- | ----- | --- | --------- | ------------ |
| code | String | Body   | true  | The code of the error |
| message | String | Body | true | The human readible error message |
| reference | String | Body | true | Reference where to find more information on error |
| Trace-Id      | String  | Body | true | Unique id generated for every request |
| status | String | Body | true | https error code, 4xx or 5xx |
| category | String | Body | true | Category of error. Values: BAD_REQUEST, FORBIDDEN, INTERNAL_SERVER_ERROR, BACKEND_ERROR, more generic categories can be found on the Get Started page under the section Error Codes. |

##### Error Codes

This section lists the errors that are specific for this API. If your error is not listed here, or you want to know which general errors can occur, you can check on the Get Started page under the section Error Codes.

| HTTP status code | Error Code | Error Description |
| ---------------- | ---------- | ----------------- |
| 400 | MESSAGE_BAI561_0017 | Invalid initiating party account number |
| 400 |	MESSAGE_BAI561_0018 | Invalid counter party account number |
| 400 |	MESSAGE_BAI561_0019 | Initiating Party and counterparty account numbers are same |
| 400 |	MESSAGE_BAI561_0022 | Remittance information has invalid character(s) |
| 400 |	MESSAGE_BAI561_0023 | Counter party name has invalid character(s) |
| 400 |	MESSAGE_BAI561_0024 | Amount is negative or zero |
| 400 |	MESSAGE_BAI561_0043 | Currency is not "EUR" |
| 400 |	MESSAGE_BAI561_0060 | Counterparty account number is blank |
| 400 |	MESSAGE_BAI561_0061 | Counterparty account number length is greater than 34 |
| 400 |	MESSAGE_BAI561_0062 | Counterparty account number is not alphanumeric |
| 400 |	MESSAGE_BAI561_0063 | Amount is mandatory |
| 400 |	MESSAGE_BAI561_0064 | Amount is invalid |
| 400 |	MESSAGE_BAI561_0065 | Invalid reference id |
| 400 |	MESSAGE_BAI561_0066 | Invalid request body |
| 400 |	MESSAGE_BAI561_0067 | Mismatch TransactionID call and token |
| 400 |	MESSAGE_BAI561_0068 | Error occurred when status for the payment is other than STORED |
| 400 |	MESSAGE_BAI561_0070 | Counter party account number has length more than 70 |
| 400 |	MESSAGE_BAI561_0071	| Counter party account number is BLANK |
| 400 |	MESSAGE_BAI561_0072	| REMITTANCE info field length is more than 140 characters |
| 400 |	MESSAGE_BAI561_0073	| Account number type is invalid |
| 400 |	MESSAGE_BAI561_0075	| Acccount number type not present |
| 400 |	MESSAGE_BAI561_0076	| Currency is mandatory |
| 400 |	MESSAGE_BAI561_0077	| Type is invalid |
| 400 |	MESSAGE_BAI561_0078	| Bank indentifier type is mandatory |
| 400 |	MESSAGE_BAI561_0079	| Counterparty missing in body |
| 400 |	MESSAGE_BAI561_0080	| Invalid chargesbearer |
| 400 |	MESSAGE_BAI561_0081	| Counter party BBAN is invalid |
| 400 |	MESSAGE_BAI561_0084	| Bank identifier type not provided |
| 400 |	MESSAGE_JA001_0001 | Client Id is blank |
| 403 | MESSAGE_BAI561_0044	| Token does not contain account number |
| 403 | MESSAGE_BAI561_0045	| Mismatch account number in call and token |
| 403 | MESSAGE_BAI561_0046	| Wrong scope |
| 403 | MESSAGE_BAI561_0047	| Scope unknown or missing |
| 403 | MESSAGE_BAI561_0048	| Wrong granttype for token |
| 403 | MESSAGE_DAO375_0004 | Date is out of range |
| 403 | MESSAGE_DAO375_0005 | Insufficient grant level |
| 403 | MESSAGE_DAO375_0006 | No access to account |
| 403 | MESSAGE_DAO375_0007 | Client not found |
| 403 | MESSAGE_DAO375_0008 | No access to account |
| 403 | MESSAGE_DAO375_0011 | Grant type missing or unknown |
| 404 | MESSAGE_BAI561_0028	| Wrong input |
| 404 | MESSAGE_BAI561_0030	| No payment details found |
| 5xx | MESSAGE_xxxxxxxxxxx | For any 500 error please contact the bank |

#### Characterset {#characterset}

For SEPA payments and crossborder payments the characterset in the table below can be used. 

| Character set |
| -------------- |
| space |
| ! & ' ( ) +   - . / 0 1 2 3 4 5 6 7 8 9 : ? _ ` , |
| aAbBcCdDeEfFgGhHiljJkKlLmMnNoOpPqQrRsStTuUvVwWxXyYzZ |
| àÀáÁâÂãÃäÄåÅæÆçÇèÈéÉêÊëËìÌíÍîÎïÏðÐñÑòÒóÓôÔõÕöÖ×øØùÙúÚûÛüÜýÝþÞßÞÿ |

**Note:** For cross border, characters may be converted (e.g. the ":" and "‘") and lines can be truncated because of differences in standards between local payment and clearing system.
