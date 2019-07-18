### Overview

#### Introduction {#introduction}
This api accepts International Bank Account Number(IBAN) as input and provides corresponding Business Contact(BC) Number/Customer Number

#### Gaining Access {#gaining-access}
To gain access to the Agreements API, you have to:

- [Register](/user/register) and create an account
- [Login](/user/login)
- [Create an app](/user/me/apps), subscribe to the Agreements API Product and get API credentials
- Authenticate yourself by following the instruction below

#### Authentication
To use any of the Chargeable Activities API operation, you need to authenticate yourself. The authentication is done on the basis of a acces token. To obtain a acces token, you need to call the PING OAuth API.

#### OAuth API (Client credentials based)
The OAuth API has one endpoint. By making a post request to this endpoint, you can obtain an acces token for the Chargeable Activities API.

#### Create Ping Token Request
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
  -d 'grant_type=client_credentials&client_id={your_id}&scope=chargeableactivities'
```