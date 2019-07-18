### Overview

#### Introduction
Account Information are a set of APIs can be used by third party payment service providers (TPP) to retrieve information for ABN AMRO accounts. Account information can only be accessed when the ABN AMRO account holder authorizes this access through the consent application. For more information see the [PSD2 section](#psd2).

A sandbox environment is available for development and testing. Access to production can be requested if your organization has a PSD2 or banking license.

#### Get started
To get started:

1. Register and create an account on the Developer Portal
2. Login
3. Create an app, subscribe to the Account Information API Product and get API credentials
4. Get access token, see Authorization
5. Start developing. See Sandbox, Technical and Tutorials sections

Let us know what you think about our API's and provide us with [feedback](https://developer.abnamro.com/contact). 

#### Get Access to Production
> Access to production can be requested if your organization has a PSD2 or banking license. For more information check the [PSD2 section](#psd2)

Follow these steps to get access to production:

To get access to our API’s an PSD2 compliant EIDAS QWAC SSL certificate is required. 

**1. Contact us**
Request access to ABN AMRO PSD2 APIs using the Contact us form. The ABN AMRO API services team will contact you to discuss the details of your request.

**2. Send setup form**
You need to fill a technical setup form and send this together with your QWAC EIDAS SSL certificate to ABN AMRO.

**3. Setup**
When we have validated the technical setup form and EIDAS certificate we will start the onboarding process for the PSD2 APIs.

**4. Go Live**
The API Services team will provide you with details for production access.

#### PSD2
If you require access as a TPP to ABN AMRO accounts you must have a PSD2 license from a local competent authority. In the Netherlands, this is De Nederlandsche Bank (DNB). If you require access to accounts in other countries you must have a license in that country as well. You can get access to ABN AMRO accounts in NL, GB, BE, and DE through the Developer Portal.

**Licensing requirements**
| API | AISP License | PISP License | Banking License | Payment Instrument Issuing License | 
| --- | :---: | :---: | :---: | :---: | 
| Account Transactions | Y | N | Y | N | 
| Account Balance      | Y | N | Y | N | 
| Account Details      | Y | N | Y | N | 
| CAF                  | N | N | Y | Y | 
