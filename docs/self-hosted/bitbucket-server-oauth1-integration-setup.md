# Bitbucket Data Center / Server Integration Setup (OAuth 1)
## Private Packagist Self-Hosted

<div class="row column">
    <div class="callout success">
        <p>
            This guide explains how to set up an OAuth 1 integration for Private Packagist Self-Hosted with Bitbucket Data Center / Server.
            If you use their public service on bitbucket.org, these instructions are not relevant to you. 
        </p>
        <p>If you are using our Cloud product at packagist.com, please <a href="/docs/cloud/bitbucket-server-oauth1-integration-setup.md">use this guide</a>.</p>
    </div>
</div>

<div class="row column">
    <div class="callout warning">
        <p>
            Note: As of Bitbucket Data Center / Server v8 Application Links using OAuth 1 can no longer be used to set up synchronizations in Private Packagist.
            Consult the <a href="/docs/self-hosted/bitbucket-server-integration-setup.md">Bitbucket Data Center / Server OAuth 2 setup guide</a> to set up an Application Link using OAuth 2 instead.
            <br>
            If you have an existing OAuth 1 Application Link, create an OAuth 2 Application Link and edit the existing integration in Private Packagist.
        </p>
    </div>
</div>

To allow your users to authenticate to Private Packagist with their Bitbucket Data Center / Server account, you'll
first need to create an integration in Private Packagist. This document walks you through the required steps.

## Open the Add Integration form
Go to the Admin section and hit the “Add Integration“ button to open the form to create your integration. If you've just
installed Private Packagist Self-Hosted and it is still running in Setup Mode, you do not need to log in. If the application
is no longer in Setup mode, you will have to log in with an admin account first.

![Add Integration](/Resources/public/img/docs/self-hosted/08-integration-create.png)

To setup a Bitbucket Data Center / Server integration with Private Packagist, start by selecting "Bitbucket Data Center / Server"
as the platform, enter the URL of your on-premise Bitbucket Data Center / Server into the base URL field and select OAuth 1 as the OAuth version as seen in the example below.

![Packagist Setup](/Resources/public/img/docs/integration-setup/self-hosted/bitbucket-server-oauth1-01-packagist-setup.png)

Submit the form to see the additional information.

![Packagist Form](/Resources/public/img/docs/integration-setup/self-hosted/bitbucket-server-oauth1-02-packagist-form.png)

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

## Finish the Setup
The integration is now ready to be used and is shown in the list of integrations on the admin page.

![Packagist Finalize](/Resources/public/img/docs/integration-setup/self-hosted/bitbucket-server-oauth1-08-integrations-overview.png)

## Configure Bitbucket Data Center / Server plugins

In case the U2F & TOTP plugin by Alpha Server is installed on the Bitbucket Data Center / Server then you will need to enable the OAuth whitelist
otherwise Private Packagist won't be able to authenticate with the Bitbucket Data Center / Server.

![Bitbucket Data Center / Server TFA Configuration](/Resources/public/img/docs/integration-setup/bitbucket-server-09-tfa.png)
