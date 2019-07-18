### Overview

#### Introduction {#introduction}
Payment Initiation are a set of API's that can be used by third party payment service providers (TPP) to initiate payments from ABN AMRO accounts. A payment can only be intiated from an account when ABN AMRO's client authorizes this payment through the consent application. Please also check [the PSD2 section](#psd2).

A sandbox environment is available for development and testing. Access to production will be available before June 14th and can be requested if your organization has a PSD2 or banking license.

#### Getting Access {#getting-access}
> Payment Initiation is available to TPP's. Please check [the PSD2 section](#psd2) for details.

#### Development and Testing {#development-testing}
You can start developing now using our documentation.

This is how to get started:

- [Register](/user/register) and create an account on the Developer Portal 
- [Login](/user/login)
- [Create an app](/user/me/apps), subscribe to the Payment Initiation API Product and get API credentials
- Check [Authorization](authorization) on how to get access tokens
- Start developing using the [Sandbox](sandbox), [Technical](technical) and [Tutorials](tutorials) sections

Please let us know what you think about our API's and provide us with [feedback](https://developer.abnamro.com/contact). 

#### Get Access to Production {#production}
> Access to production will be available before June 14th and can be requested if your organization has a PSD2 or banking license. Please check [the PSD2 section](#psd2) for details.

Follow the steps below to get access to production:

**1. Contact us**
You can request access to PSD2 API's using the [Contact us](/contact) form. Our API services team will contact you to discuss details of your request. They will ask you to provide information that will help us to determine your identity and authorizations for PSD2 access.

**2. Request a certificate**  
To get access to our API’s an EIDAS SSL certificate is required. 

**3. Send documents**
After we have validated your identity and authorizations for PSD2 access, we will send you setup documents/forms. These will need to be signed by authorized representatives of your organization.

**4. Setup**
Once we have validated your identity and PSD2 permissions and have received the duly signed documentation, we will start the onboarding for the PSD2 API's.

**5. Go Live**
After you completed testing of your application we will activate access to production. Our API Services team will help you going live.

#### PSD2 {#psd2}
When you require access as a TPP to ABN AMRO accounts you need a PSD2 license from a local competent authority. In the Netherlands, this is DNB. If you require access to accounts in other countries you need a (passported) license in that country as well. You can get access to ABN AMRO accounts in NL, GB, BE and DE through the Developer Portal. Access to production will be available before June 14th.

Please check the table below for the required license:

| API | AISP License | PISP License | Banking License | Payment Instrument Issuing License |
| --- | :------------: | :------------: | :--------: | :--------------------------: |
| Payments        | N | Y | Y | N |
