**Provision an Instance**

**Introduction**

It is often a requirement for organizations to on-board an employee or contractor or some identity manually into a central identity system like IDCS. In this use case, an IDCS user administrator will manually add a new user in IDCS service. Typically, this would occur through automated provisioning, bulk flat-file import or synchronization with your on-premises Active Directory.

The IDCS features are built 100% using the IDCS REST API service. Therefore, any task we perform interactively in the web interface could also be performed via custom app, using these same REST API calls. It’s a clear advantage of Oracle’s API-first architecture with IDCS.

IDCS supports user (also groups) on-boarding from on-premises Active Directory, using file upload, REST API, on-premises Oracle Identity Management solution, or manually from IDCS admin console. For the workshop we will be using file upload option and API calls user management.

Estimated Lab Time: 90 minutes

**Objectives**

In this lab, you will:

-   create User in UI
-   import Users with CSV
-   create users using REST API’s

**Prerequisites**

-   An Oracle Free Tier or Paid Account
-   Postman native application installed locally

Collapse All Tasks

**Task 1: Create User in UI**

-   *Personas*:
    -   Administrator
1.  Go to IDCS Admin console using your administrator account credentials. Select the *Users* menu on the left and click *+Add* or select the *Add a user* icon from the dashboard.

    ![Image](media/2728f66b2ca5bd41af79c143df9cd799.png)

2.  Fill in all required fields and click Finish. As a good practice, use a consistent naming convention throughout the labs. For example:
    -   First Name: Firstname
    -   Last Name: Lastname-UI
    -   User Name/Email: firstname.lastname+ui@domain.tld

        ![Image](media/f86a54e8e73ac4f381ed556dc5e7f4d2.png)

3.  Verify user creation. In order to do that, go to the *Users* tab in admin console. Verify that the new users are visible on the console.

    ![Image](media/e792b28a0d39cb280bb8631d5b7c914e.png)

    Once a new user is created in the IDCS service, the user receives an email invitation to finish first time login formalities such as setting a password to logging into IDCS.

**Task 2: Import Users with CSV**

-   *Personas*:
    -   Administrator

If you are an identity domain administrator or a user administrator, you can batch import user accounts using a comma-separated values (CSV) file.

1.  Obtain upload CSV file. In order to do that, select the *Users* menu on the left and click *Import*. In the Help section on the right, click *Download sample file*.

    ![Image](media/f9452cf1f9b69899388c7354eacd12a5.png)

    **Alternatively**, you can export a list of existing users: filter the user created in the UI, then click on Export-\>Export All. Confirm the export then go to the Job and download the file

    ![Image](media/5909be0fa1f0a0178e18294ccdd68a9b.png)

    Extract the zip file and open *Users.csv* or open the *UserExport… .csv* file. Inspect the content of the file from your favorite editor. You can use the default examples that you find into the csv file or you can fill in some more if you want but keep it similar to the examples. For example, if you use the exported existing user, modify the suffix (UI) to another one (e.g CSV).

2.  Go to IDCS Admin console using your administrator account credentials. Select the *Users* menu on the left or select the *Users* icon from the dashboard.

    ![Image](media/b5419d896ec5531b6ea149c66c90820a.png)

3.  Click on the *Import* button.

    ![Image](media/cd2daef59a67e573a4d543e19e066175.png)

4.  The Import Users screen pops up. Click on *Browse*. Select the CSV file. Click on *Import*.

    ![Image](media/a856b405e28d7c889b14193b82e8f32d.png)

    By uploading the sample file, sample users will be uploaded into IDCS. In order to upload different users, the csv sample file needs to be altered.

5.  Go to the *Jobs* menu item and verify that the import Job finished successfully. Click on *View Details*. to check if all the users were imported. You’ll also be presented with the list of all the users from csv, together with all the attributes details and status of creation.

    ![Image](media/3f043e567e13ca5be19d861ec797750b.png)

6.  Verify user creation by going to the *Users* tab in admin console. Verify that the new users are visible on the console.

    ![Image](media/f84574cf22e269a5eb5e5245645c3d35.png)

7.  Click on your target end-user (e.g. Firstname Lastname-CSV) and verify user's detailed attribute information
8.  Just for fun, change the email for the user to one you control and select *Update User*. Then click the *Reset Password* button.

    ![Image](media/8de3b43888e8946a6d26477be9578233.png)

9.  Check your email for the reset message and change the password.

    ![Image](media/661d7ff1c0bceac036306c5aa2ce16cf.png)

10. Then, open another browser (if using Chrome, select Incognito Window) and login to your IDCS Admin Console as an administrator:

    https://\<your tenant\>/ui/v1/adminconsole

    ![Image](media/dfe2e8b906c9a437a5353c0d2c6711d7.png)

    You will see that the user has no access to applications yet. In Lab 2, we will show you how a user can request applications.

    ![Image](media/9cb20f669f6a20c7f86c2400be0d2e2d.png)

**Task 3: API User Creation with REST APIs**

-   *Personas*:
    -   Administrator

This use case involves making API calls to IDCS using a REST client; in this case Postman.

The Postman collection of relevant REST API calls is provided to each participant.

This use case will demonstrate using a common utility, Postman, for connecting to Identity Cloud Service via the out-of-the-box REST API.

**Register a client POSTMAN application in IDCS**

1.  Login to your IDCS Admin Console as an administrator:

    https://\<your tenant\>/ui/v1/adminconsole

2.  Select the *Applications* tab from the IDCS dashboard presented after log in

    ![Image](media/752edc74ab5a089ce78afb2d797018db.png)

3.  Click the *Add* button to create a new application for Postman use. In order for Postman to be able to call IDCS REST APIs, it first requires having a CLIENT*ID and CLIENT*SECRET that authorize Postman to do so. This can be achieved by creating a Confidential Application type into IDCS with specific authorization grant types as you will see into the following steps.

    ![Image](media/035a04e76b777d38ef69b03494111143.png)

4.  Select *Confidential Application* from the pop-up menu of application types

    ![Image](media/97352d82ce90ffd1bf4e12c1a4fc80c7.png)

5.  Set the *Name* to "Postman-\<\>"" then click *Next*

    ![Image](media/d74a62b813a575b019106e5d6ae8a54d.png)

6.  Click *Configure this application as a client now* in order to provide the authorization grant types and the API role.

    ![Image](media/8d3dbbc94eb594cbef12f823fa5de082.png)

7.  Select all the *Allowed Grant Types* checkboxes and set *Redirect URL* to https://localhost. Go to *Grant the client access to Identity Cloud Service Admin APIs* section (bottom of the page) and add the following API role *Identity Domain Administrator*. This way you are basically granting access for this IDCS app to the full set of IDCS APIs. Then, click *Next*.

    ![Image](media/9f4561a6bc04a24b964b31455fb32848.png)

8.  Make no changes in *Resources* and click *Next*

    ![Image](media/c9ee4885ee49bb55350979707047fde4.png)

9.  Make no changes in *Web Tier Policy* and click *Next*.

    ![Image](media/0ac700c62d42fb5e05f2c0dd0305c14b.png)

10. For finalizing the IDCS app that grants API roles to Postman via Auth2.0 standard click *Finish*.

    ![Image](media/40490251e13257c2192eaa398b35de47.png)

11. Once the application is created, note down the *Client ID* and the *Client Secret* then click *Close*. These will be used by Postman desktop application to call IDCS APIs using the Auth2.0 standard protocol.

    ![Image](media/719ab140ed07753093eb6bf409cd6017.png)

12. Click the *Activate* button.

    ![Image](media/da6c2005f87d9fe5ed8694b4cdbb836a.png)

13. Confirm the application activation

    ![Image](media/e93763b948f5d16146ff2d74d74ef539.png)

14. The application is now active and ready to use.

    ![Image](media/35e277dba045fd6cd564a38b01ff2be7.png)

15. Sign out from IDCS

**Configure Postman**

1.  Open Postman. Ignore all startup messages if any.

    ![Image](media/2ca382f2214f09a2bf26c6dce42d86d1.png)

2.  First we need to import IDCS Postman environment variables, global variables and IDCS API collection. Click the *Import* button on the left upper corner.

    ![Image](media/633b8b58c6630fe253b94659deef37f2.png)

3.  Select *Import from Link* and provide [Example Environment](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/example_environment.json) URL into the box to import the environment variables and click *Import*

    ![Image](media/b7c5de949261f398b606f83278ba6ec6.png)

4.  To import the Oracle Identity Cloud Service REST API Postman collection, on the Postman main page, click *Import* again.
5.  In the Import dialog box, select *Import From Link*, provide the [IDCS Postman collection](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/REST_API_for_Oracle_Identity_Cloud_Service.postman_collection.json) URL into the box, and then click *Import*.
6.  The left pane of Postman has a sub-tab called Collections.

    With Collections selected, you will see the various REST statements which have been pre-configured for today’s workshop.

7.  To import the global variables file, click *Import*.
8.  In the Import dialog box, select *Import From Link*, provide the [IDCS global variables](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/oracle_identity_cloud_service_postman_globals.json) URL into the box, and then click *Import*.
9.  Select the *Oracle Identity Cloud Service Example Environment with Variables* environment then click on the *Settings* button (cogwheel icon) to *Manage Environments*

    ![Image](media/300375318d09c07363785cf7e44ac775.png)

10. Click on the newly created environment which will be like *Oracle Identity Cloud Service Example Environment with Variables* - it will be the only one available in your cases on a fresh Postman installation.

    ![Image](media/7549d10fe7d005d6df12a3da4545a96d.png)

11. Set the following parameters initial and current values in order to be able to obtain an IDCS access token:

| Table 1: Oracle Identity Cloud Service Introductory Workshop \| Lab 1: User Management |                                                                                                 |
|----------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| **Parameter**                                                                          | **Value**                                                                                       |
| HOST                                                                                   | Your IDCS tenant URL Ex: https://idcs-8b000000000000000000000000000000.identity.oraclecloud.com |
| CLIENT_ID                                                                              | Postman IDCS Client application CLIENT_ID                                                       |
| CLIENT_SECRET                                                                          | Postman IDCS Client application CLIENT_SECRET                                                   |

1.  ![Image](media/d19d8a31c97423fea7454a3c04a0fa7a.png)
2.  Click on *Update* once completed.

**Request an Access Token**

The steps performed above are being done to obtain the Access Token, which is shown in the following screen shot.

There are several types of OAUTH requests supported by IDCS, including client credentials. With a client credentials request, the client ID and client secret are used by the calling application to identify itself and to request an Access Token. The Access Token is available for use by the application until the time that it expires.

This Access Token establishes trust between your application and IDCS.

1.  On the *Collections* tab, expand *OAuth*, and then *Tokens*.
2.  Select *Obtain access_token (client credentials)*, and then click *Send*. The access token is returned in the response from Oracle Identity Cloud Service.

    ![Image](media/db00e752e033209e1be214de661645b0.png)

3.  Validate if you can see status *200 OK*.![Image](media/5995b739a90d4b48051cc835b72a552d.png)
4.  Highlight the access token content between the quotation marks, and then \*right-click(). In the shortcut menu, select \*Set: Oracle Identity Cloud Service Example Environment with Variables\* In the secondary menu, select *access_token*. The highlighted content is assigned as the access token value.

    ![Image](media/c4fd8aaf84df5655c568371519053308.png)

**Create an IDCS User via API**

1.  On the *Collections* tab, expand *Users*, and then *Create*.
2.  Select *Create a user*. The request information appears with some default values as can be seen into below print screen. You can either change them as per your preference or you can simply use the default filled values. For example:
-   givenName: Firstname
-   familyName: Lastname-REST
-   username/email-\>value (x2): firstname.lastname+REST@domain.tld
1.  Click *Body*, and then click *Send*.

    ![Image](media/89895d2d72b287e483b7e567aaa171f1.png)

2.  In the response, confirm that the status *201 Created* appears and that the response body displays details about the user that was successfully created in Oracle Identity Cloud Service.
3.  While in the IDCS UI you can see the user as being created.

    ![Image](media/538fae7e8e6e893cfb0920ec7aea8004.png)

You can also *modify* and *delete* users and a lot more via REST API. For more information, please check the documentation below.
