# digital-theater-plus-google-sso
How to set-up SSO in G Suite for integration with Digital Theatre+ (DRAFT)

## Goal
Identify the steps required for an IT administrator to setup G Suite for SSO as SAML identity provider, for authentication of G Suite users to access Digital Theatre+

### Step 1: Organisation IT administrator to provide the metadata to Digital Theatre+ configuration.

1. Login to https://admin.google.com using your administrator account.
2. Click Security > Set up single sign-on (SSO).
3. Click the Download button to download (a) the Google IdP metadata and (b) the X.509 Certificate.
4. Click on Apps > SAML apps.
5. Select the Add a service/App to your domain link or click the plus (+) icon in the bottom corner. This opens the Enable SSO for SAML Application window.
6. Click SET UP MY OWN CUSTOM APP
7. As we have already downloaded the certificate and Idp Metadata, click NEXT
8. On the Basic application information window, Enter the Application name and Description values.
9. In the Service Provider Details section, enter the following URLs into the Entity ID, ACS URL, and Start URL Fields:
* ACS URL: https://www.digitaltheatreplus.com/Shibboleth.sso/SAML2/POST
* Entity ID: https://www.digitaltheatreplus.com
* Start URL: Leave blank
* Leave Signed Response unchecked.
* The default Name ID is the primary email and select EMAIL as Name ID Format.
* Ignore Adding attributes mapping as DT+ does not need any attributes and does not store any user info locally.
10. Click Finish.
11. Click on Apps > SAML apps and select your APPLICATION.
12. At the top of the gray box, click More and choose:
* On for everyone to turn on the service for all users (click again to confirm).
* Off to turn off the service for all users (click again to confirm).
* On for some organizations to change the setting only for some users.

### Step 2: Digital Theatre+ add shibboleth configuration for the organisation

Google does not provide metadata URLs for G Suite so Digital Theatre+ will host the meta data on an AWS S3 bucket with a publicly accessible URL.

1. Upload the provided metadata to S3 bucket (https://dtpserverconfiguration.s3-eu-west-1.amazonaws.com/google-metadata).
2. Set S3 access policy to public
3. Download https://dtpserverconfiguration.s3-eu-west-1.amazonaws.com/shibboleth2.xml
4. Add an entry to the bottom for the new Identity Provider
5. Rename the existing shibboleth2.xml file above on dtpserverconfiguration by adding date tag in it.
6. Upload the updated shibboleth2.xml file to dtpserverconfiguration
7. Set the public access to the file
8. Make the same changes to the file shibboleth2.xml file in code repository.
9. Deploy the change.

Caveat: There may be some variations based on individual organisation's SSO setup. For Google, Digital Theatre+ will be hosting the metadata. If the metadata is changed by Google then it will need to be updated on Digital Theatre+ as well.
