### Authorization

**Important:** To use our API you first need to be onboarded. If you haven't been onboarded please check our onboarding procedure in Overview.

ABN AMRO client accounts are protected against unauthorized access. You must authenticate yourself using OAuth2.0 and get authorization to the client's account. For more information you could have a look at the OAuth pages from the Internet Information Task Force.

**Note:** To use our API you first need to be onboarded. If you haven't been onboarded please check our onboarding procedure in Overview.

#### Authorization code {#authorization-code}

This OAuth authorization is used in a TPP role to access the accounts of ABN AMRO clients. ABN AMRO clients must provide consent to you as a TPP before the account can accessed.

An ABN AMRO client must be requested to provide access through the [consent application](#consent-app) to you to access account information. As result of this consent an "authorization code" is provided. 
When consent is granted an "authorization code" is provided. For security reasons this code is short-lived and must be exchanged for a long-lived "refresh token" and a short-lived "access token". This "access token" can be used to get access to the APIs (e.g. release the stored payment for processing).
The long-lived refresh token can be used for future account access, see the section "refresh acces code token" for details.  The sequence diagram below depicts this flow.

For details on how to access the OAuth server, see OAuth or Tutorials.

#### Consent Application {#consent-app}

The consent application is used in the authorization code process to provide you with an access code for the requested authorization. 
Authorization is a grant to an account for one or multiple scopes. A scope defines the type of access. For account information there are different scopes that can be requested. For example a scope to access the balance of an account.
In the consent application the ABN AMRO client can grant you access to their account, by using for example an E.dentifier. This is a so-called redirect. In the consent application, the ABN AMRO client can review the access to their account that you request. Also they can select to which account they want to provide access. The ABN AMRO client can either authorize or cancel the requested authorization.

The client consent process consists of 3 steps:

1. Logon
2. Check requested access to account
3. Authorization

All account information consents are valid for 90 days. Consent can be given using Internet Banking, Mobile Banking app or Access Online.

**Notes:**

- The scopes for Account Information cannot be combined with scopes for Payment Initation. See Technical.

- For details on how to access the consent application through the OAuth server, see OAuth or the Tutorials.

- The ABN AMRO client can select either Dutch or English as languague in the consent application.

- A cookie is used in the consent process that must be stored in the browser of the ABN AMRO client.

#### Refresh an Access Token {#refresh-access-token}

When the short-lived access token has expired, the long-lived "refresh token" can be used to get a new access token and a new refresh token. This renders the used refresh token as invalid. The sequence diagram below describes this process.

For details on how to access the OAuth server, please check OAuth or the Tutorials.

#### Consent Info API {#consent-info-api}
Use this method to retrieve the details (account number and scopes) of consent that are associated with an "access token".

**Note:** For details on how to access the Consent info API, please check Technical or the Tutorials.