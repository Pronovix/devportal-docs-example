### Technical Details

#### Open API Specification {#open-api-spec}
Click [here](/system/files/documents/Agreements-v1.yaml) to download the Open API Specification of Agreements API in yaml format. 

#### Postman Collection
Click [here](/system/files/documents/agreements-v1.postman_collection.json) to download the postman collection of Parties API in JSON format.

#### Authentication
ABN-AMRO only allows clients to use TLS-MA for authentication this means that you need to have a certificate. ABN-AMRO requires a certificate to be with any of these certificate authorities :

1. DigiCert
2. QuoVadis
3. VeriSign

The certificate needs to be registered with ABN-AMRO (only the public part) this is part of the on-boarding process. After the registration of your certificate with ABN-AMRO, The ABN AMRO Ping Team can create your client and authorize you for the required scopes. When this is done we will provide you with your client-id and with this you can go and request tokens.

For further queries and support please contact us.

#### Acces token parameters {#token-parameters}
ABN-AMRO only allows clients to use TLS-MA for authentication as described [here](/get-started#authentication) for authentication. To use the  API, you need to obtain a acces token from PING with the following scope:

 | Key   | Value  |
 |-------|--------|
 | scope | agreements:bcnumber:read |

#### Create Ping Token Request

## Access Token Endpoints
    Sandbox: https://security-ifs-auth-et.connect.abnamro.com
    Production: https://security-ifs-auth.connect.abnamro.co


## Samnple request
To generate the acces token from PING, you can use the following sample API call.
```
curl -X POST \
  https://security-ifs-et.connect.abnamro.com/as/token.oauth2 \
  -v '\
  -cert  '{location_of_your_certificate}' \
  -key  '{location_of_your_private_key}' \
  -cacert  '{location_of_your_ca_certificate_chain}'  \
  -H 'Accept: application/json' \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id={your_id}&scope=agreements:bcnumber:read'
```

If the request is successful, you can expect a 200 ok, and in the body it returns a string (customerId) plus a unique id generated for every request, as shown in the example below:
```
{
    "customerId": 43181554
}
```

##### Error Attributes  
This section lists the errors for this API. If your error is not listed here, or you want to know which general errors can occur, please check the [general error section](/get-started#error-codes).

| Code | Status | Category | Description |
| --- | --- | --- | --- |
| ERR_1101_001 | 400 | BAD_REQUEST | Mandatory parameter consumerSystem is missing or null |
| ERR_3100_001 | 403| FORBIDDEN	Maximum number of platforms per client reached |
| ERR_3100_002 | 403 | FORBIDDEN Maximum number of users has been reached for this platform|
| ERR_4100_001 | 404 | NOT_FOUND The client could not be found/does not exist|
| ERR_4100_002 | 404 | NOT_FOUND The platform could not be found/does not exist|
| ERR_4100_003 | 404 | NOT_FOUND The user could not be found/does not exist|
| ERR_8100_001 | 500 | INTERNAL_SERVER_ERROR An unknown error occurred in the backend. Please contact us if this problem persists | 
| ERR_9100_011 | 503 | SERVICE_UNAVAILABLE Tikkie service unavailable |

