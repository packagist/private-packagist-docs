# Bitbucket Data Center / Server Integration Setup (OAuth 1)
## Private Packagist Cloud

<div class="row column">
    <div class="callout warning">
        <p>
            Note: As of Bitbucket Data Center / Server v8 Application Links using OAuth 1 can no longer be used to set up synchronizations in Private Packagist.
            Consult the <a href="/docs/cloud/bitbucket-server-integration-setup.md">Bitbucket Data Center / Server OAuth 2 setup guide</a> to set up an Application Link using OAuth 2 instead.
            <br>
            If you have an existing OAuth 1 Application Link, create an OAuth 2 Application Link and edit the existing integration in Private Packagist.
        </p>
    </div>
</div>

## Initial Setup

From the organization settings page, select the "Integrations" subtab.
Hit the "Add Integration" button on the integrations listing page to get to the form below.
To set up a Bitbucket Data Center / Server integration with Private Packagist, start by selecting "Bitbucket Data Center / Server"
as the platform, enter the URL of your on-premise Bitbucket Data Center / Server into the base URL field and select OAuth 1 as the OAuth version as seen in the example below.

![Packagist Setup](/Resources/public/img/docs/integration-setup/cloud/bitbucket-server-oauth1-01-packagist-setup.png)

Submit the form to see the additional information.

![Packagist Form](/Resources/public/img/docs/integration-setup/cloud/bitbucket-server-oauth1-02-packagist-details.png)

## Configure Application link
Click on the link to setup an Application Link on Bitbucket Data Center / Server. You will need the "Client Id" and the "Public Key" shown in the form.

![Bitbucket Data Center / Server Configure Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-oauth1-03-bitbucket-configure-application-link.png)

For Bitbucket Data Center versions 7.21 and newer, select "Atlassian product" and not "External application" to be able to set up an OAuth1 link.

After you hit the button to create a new Application Link a configuration window may appear. If it does, verify that the url matches your Private Packagist URL and hit "Continue", otherwise skip this step.

![Bitbucket Data Center / Server Invalid Url](/Resources/public/img/docs/integration-setup/bitbucket-server-oauth1-04-bitbucket-invalid-url.png)

Now setup a "Generic Application". The only field required is the "Application Name". Submit the form to finish creating the Application Link.

![Bitbucket Data Center / Server Setup Link](/Resources/public/img/docs/integration-setup/bitbucket-server-oauth1-05-bitbucket-setup-link.png)

![Bitbucket Data Center / Server Application Created](/Resources/public/img/docs/integration-setup/bitbucket-server-oauth1-06-bitbucket-application-created.png)

Click on the pen icon to the right of the application you just created to edit the Application Link and configure Incoming Authentication.

![Bitbucket Data Center / Server Incoming Auth](/Resources/public/img/docs/integration-setup/bitbucket-server-oauth1-07-bitbucket-incoming-auth.png)

This is where we will need the "Client ID" and the "Public Key" that were previously generated on the Private Packagist integration form.
Enter the "Client ID" into the "Consumer Key" field, make sure the entire content of the "Public Key" field gets copied and the "Consumer Callback" field stays empty.
Submit the form and go back to Private Packagist.

## Configure Bitbucket Server plugins

In case the U2F & TOTP plugin by Alpha Server is installed on the Bitbucket Data Center / Server then you will need to enable the OAuth whitelist
otherwise Private Packagist won't be able to authenticate with the Bitbucket Data Center / Server.

![Bitbucket Data Center / Server TFA Configuration](/Resources/public/img/docs/integration-setup/bitbucket-server-09-tfa.png)
