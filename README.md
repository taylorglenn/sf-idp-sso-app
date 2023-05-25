## Enable "My Domain" on your Salesforce ORG

From **Setup > Domain Management > My Domain** enable My Domain on your org.

Remember that once set you cannot change it anymore.


## Enable Salesforce as Identity Provider

From **Setup > Security Controls > Identity Provider** click **Enable Identity Provider**.


## Create a connected app

From **Setup > Create > Apps** click on **New** button of the **Connected Apps** section.

In **Web App Settings** set:

* *Start URL*: url of your app (e.g. http://localhost:3000)
* *Enable SAML*: true
* *Entity Id*: service provider entity id (e.g. passport-saml, this is the issuer field on the Service Provider as seen more over)
* *ACS Url*: callback (e.g. http://localhost:3000/login/callback)
* *Subject Type*: field used to match users (use User ID)
* *Name ID Format*: format of the field used to match (e.g. urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified)
* *Issuer*: leave your domain (e.g. https://blog-domain-dev-ed.my.salesforce.com)

## Service Provider Configuration

Go back to **Setup > Security Controls > Identity Provider** and get the info on your **SAML Metadata Discovery Endpoints**.

Open one of the *xml* links (e.g. https://[your_domain].my.salesforce.com/.well-known/samlidp.xml).

Copy:
* Node *ds:X509Certificate*: this is the main certificate used to authenticate the communication between the Service PRovider and Salesforce
* Node *md:SingleSignOnService*: this is the URL used as an entry point for the authentication on the Identity Provider
* Node *md:NameIDFormat*: this is the format of the incoming user ID

Using the *SAML Service Provider Server* (https://github.com/enreeco/sf-idp-sso-app) service configure a new instance of the server setting the following enviromental variables:

* *SAML_ISSUER*: leave 'passport-saml' (or choose your own)
* *SAML_ENTRYPOINT*: set with the valule in the *md:SingleSignOnService* node
* *SAML_CERT*: set with the *ds:X509Certificate* node
* *SAML_IDENTIFIER_FORMAT*: set with the *md:NameIDFormat* node
