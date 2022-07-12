**Configure Federation – External Identity Provider (Okta)**

**Introduction**

An *identity provider*, known as an *Identity Assertion provider*, provides identifiers for users who want to interact with Oracle Identity Cloud Service using a website that's external to Oracle Identity Cloud Service. A *service provider* is a website such as Oracle Identity Cloud Service that hosts applications. You can enable an identity provider and define one or more service providers. Your users can then access the applications hosted by the service providers directly from the identity provider.

For this exercise, a website can allow users to log in to Oracle Identity Cloud Service with Okta credentials. Okta acts as the identity provider and Oracle Identity Cloud Service functions as the service provider. Okta verifies that the user is an authorized user and returns information to Oracle Identity Cloud Service.

Estimated Lab Time: 30 minutes

**Objectives**

In this lab, you will:

-   configure federation in Okta
-   configure federation in IDCS
-   login in to IdP

**Prerequisites**

-   An Oracle Free Tier or Paid Account
-   A Google Account
-   An Okta Integrator Account

Collapse All Tasks

**Task 1: Configure Okta**

-   *Personas*:
    -   Administrator
1.  1\. Go to Okta and signup for a [developer account](https://www.okta.com/integrate/signup/). This part was covered in the prerequisites section.
2.  Once you have your trial account credential, then login using the URL you’ve received after requesting the trial (the custom one, ex: https://oracleworkshop.oktareview.com) and go to *Applications*. In upper left menu select *Classic UI*.![Image](media/416147e61b9d66d4ceae02d45fab5c65.png)
3.  In the upper tab, select *Applications* and then *Applications*![Image](media/977b9d40d72e6949b7beecf148b84393.png)
4.  Select the *Add Application* button.![Image](media/38a04925c6f5eb9d52e0b643b35bb36e.png)
5.  Select *Create New App* button.![Image](media/e26c6d1c6506fff79877893480bf02be.png)
6.  Select *SAML2.0* and click *Create*.![Image](media/846ab3bfc091606c4df029e8cbc133af.png)
7.  Provide an application name, for example: **IDCS_SP** and click *Next*.![Image](media/fbf64d2b7e059b34007a8217cc841459.png)
8.  For the *Configure SAML* section, you need to enter values from the IDCS metadata

    https://\<your tenant\>.identity.oraclecloud.com/fed/v1/metadata

9.  Enter the following values from the metadata in Okta’s SAML setup:
    -   In the IDCS metadata, search for *AssertionConsumerService* at the bottom. Copy the highlighted URL in *Location* as shown below. Enter the URL as *Single sign on URL* in Okta![Image](media/d0038fdd64ee76b7222568940e72b6a8.png)
    -   In the IDCS metadata, search for *entityID* at the top. Copy the highlighted URL as shown below. Enter the URL as *Audience URI (SP Entity ID)* in Okta![Image](media/15250f2ca7e88f1c340c3b315c1cdbd2.png)
    -   When you’re done, the general settings in Okta should look something like the example below. Leave the rest of the fields with their default values![Image](media/d5c1f7f29d778fe6ac0ac25ebd927d4a.png)
10. Scroll down to the end of the page and click *Next*.![Image](media/c272ce07ac6371708d54df1695bf9c0a.png)
11. Provide the obligatory feedback and select *Finish*.![Image](media/19bf63b0280f6bec3639b66c5ca7df7e.png)
12. Click on *Identity Provider metadata* under *Settings* in order to export the IDP metadata file.![Image](media/462def545b62c3bbf3afa16dcae94a25.png)
13. The metadata will be displayed in another browser window. Save it to an xml file by right click and choose “Save as”, as you will need this in a few minutes inside of IDCS.![Image](media/a469ca534c104420b8999eed1f051545.png)
14. Go back to *Okta* and select the *Assignments* tab of your newly created application in order to assign users to the application.![Image](media/6b7768cf09aa2b48fd74dfe4f9b15fc3.png)
15. Click on *Assign* and select *Assign to People*![Image](media/b38e36c12c9e6052d440293c4c34f81b.png)
16. Select a user on the list and lick *Assign*. Enter the User Name of your corresponding IDCS user and select *Save and Go Back*. When the user is assigned, click *Done*.

    ![Image](media/1d9f56cc9e4c47a6f418206de320f413.png)

    Note: This is not the user name that you provide when logging in to Okta. That remains unchanged. What you’re entering here is the user name of the corresponding user in IDCS. This enables a link between accounts with different user names in IDCS and Okta. The accounts might have different user names e.g. because Okta requires them in an email format while IDCS doesn’t.

17. IDCS is now configured as a Service Provider in Okta. Open IDCS to configure Okta as an Identity Provider.

**Task 2: Configure IDCS**

-   *Personas*:
    -   Administrator
1.  Login to IDCS Admin console and go to *Security* and then click on *Identity Providers*. Click on *Add SAML IDP*. Enter the name of the provider (e.g “Okta-\<\>”) and description and click next.![Image](media/4bf40df76c34bc7cfdd20fac4215abb9.png)
2.  Upload Okta metadata xml file that was exported previously and click *Next*.![Image](media/e79dcc41f265dc2c3310c415567b96fb.png)
3.  Set the following parameters:
    -   Identity Provider User Attribute = **Name ID**
    -   Oracle Identity Cloud Service User Attribute = **Username**
    -   Requested NameID Format = **Unspecified**![Image](media/d053d4ba98ab9ea0b66ac4b309e62ef3.png)
4.  On the export screen there is nothing to do as we had already retrieved the IDCS metadata from the URL and is no need to download them again. Click *Next*.![Image](media/b7c45ee34bd31f207ab440558b7f503c.png)
5.  Select *Test Login* to check the connection between IDCS and Okta.![Image](media/3ae9739f188e7a7fa163dceb4a4245ec.png)
6.  Enter your Okta credentials.![Image](media/ac4c197b241a71a9c2fa35d4065ac48a.png)
7.  If everything works, you will see the message shown below. Otherwise, check your configurations for the IDCS application in Okta and/or the Okta SAML IDP in IDCS.![Image](media/1b461981bc42c92702318e18218f1f78.png)
8.  Back at the SAML IDP set-up flow, select *Next*.![Image](media/a61eb4764077b06f19289782bf46ffc1.png)
9.  Select *Activate* and then *Finish*. You’ve now added Okta as an external identity provider and successfully established a connection with IDCS.![Image](media/c6f8b71b2cccead34092fa1d80f16a74.png)
10. After successfully finishing the Okta application setup, you’ll be returned to the *Identity Providers* screen. Select the menu to *Show on Login Page*.![Image](media/422018802556c3b33436ae046fb3d59c.png)
11. Go to *Security*, select *IDP Policies* and click *Default Identity Provide Policy*.![Image](media/c3d0120816d9aec801ef5ad9f68689eb.png)
12. Select the *+ Assign* menu option and add the Okta Provider.![Image](media/f552437883c0b2c9cb1ac01dc17e87fc.png)
13. Select *Okta* and click *OK*.![Image](media/899194740175e5c154901760a4eb3bbd.png)
14. *Okta* will show up in the list![Image](media/73ce6e3f939eda7564b1e0362b2c1f9b.png)
15. *Sign out* of IDCS.![Image](media/fc5abd44df9e2e4549b7ffce4df0b826.png)

**Task 3: Verify External IDP Login**

-   *Personas*:
    -   Administrator
1.  Access the IDCS admin console and Okta will be displayed as a sign-in option. Select *Okta*.![Image](media/56ec48c784185af4319e25903d71094f.png)
2.  Type in your Okta login credentials.![Image](media/254beb8540b6b1865ceafd5046cb84cd.png)
3.  When successfully authenticated the request will be sent to the IDCS My Apps screen.![Image](media/da1c067ca42f30d73409cf7d2ab34dae.png)

    Note: for federation to work, you need to login with an account that is defined in both IDCS and Okta.

You may now proceed to the next lab
