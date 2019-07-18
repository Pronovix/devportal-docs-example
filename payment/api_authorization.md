### Authorization
The accounts of ABN AMRO's clients are protected against unauthorized access. You need to authenticate using OAuth2.0 and get authorization to the client's account.

> If you are a developer and unfamiliar with OAuth 2.0, you could have a look at the [OAuth pages from the Internet Information Task Force](https://oauth.net/2/)

**Note:** To use our API you first need to be onboarded. If you haven't been onboarded please check our onboarding procedure in [Overview](overview).

There are two OAuth flows in use:

- Client Credentials
- Authorization Code

#### Client Credentials
In this flow OAuth is used for direct access to an API. As third party payment service provider (TPP) you use this method to get an access token for registration of a payment. When requesting an access token you can identify yourself as a client using an EIDAS SSL Certificate. As response an access token is returned that can be used to access API's for which you have authorization. For security reasons the validity of this token is temporary. The sequence diagram in the authorization code section illustrates how client credentials are used in the context of getting access to an ABN AMRO account.

#### Authorization code {#authorization-code}
In this flow, OAuth is used in a TPP role to access accounts of ABN AMRO clients. ABN AMRO clients need to provide consent to you as a TPP before the account can accessed.

Before consent can be requested, first a payment instruction needs to be registered. Registration of payment is done using client credentials authorization. After that an ABN AMRO client can be requested to provide access through the [consent application](#consent-app) for the registered payment.
As a result of this consent an "authorization code" is provided. For security reasons this code is short-lived and needs to be exchanged for a long-lived "refresh token" and a short-lived "access token". This "access token" can be used to get access to the API's (e.g. release the stored payment for processing).
The long lived refresh token can be used for future account access, see the section "refresh acces code token" for details.  The sequence diagram below depicts this flow.

For details on how to access the OAuth server, please check OAuth or the Tutorials.

#### Consent Application {#consent-app}
The consent application is used in the [authorization code flow](#authorization-code) to provide you with an access code. In this application the ABN AMRO client can give consent for access to their account, by using for example their E.dentifier.  This is a so-called redirect. In the consent application, the ABN AMRO client can review the details payment that was registered by you and authorize this payment. The ABN AMRO client can either authorize or cancel the requested authorization (requested scopes).

The client consent flow consists of 3 steps:

1. Logon
2. Select Account
3. Authorization

All payment initiation consents are valid for 90 days. Consent can be given using Internet Banking, Mobile Banking app or Access Online.

**Note:**

- The scopes for Payment Initiation cannot be combined with scopes for Account Information. See Technical.

- For details on how to access the consent application through the OAuth server, please check OAuth or the Tutorials.

- The ABN AMRO client can select an account number that is different from the account number in the registered payment.

#### Refresh an Access Token {#refresh-access-token}
When the short lived access token has expired, the long lived "refresh token" can be used to get a new access token and a new refresh token, rendering the used refresh token as invalid. The sequence diagram below depicts this flow.

For details on how to access the OAuth server, please check OAuth or the Tutorials.

#### Consent Info API {#consent-info-api}
With this method you can retrieve the details (transactionId, initiating accountnumber and scopes) of consent that are associated with given "access token".

**Note:** For details on how to access the Consent info API, please check Technical or the Tutorials.