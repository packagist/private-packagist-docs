# Bitbucket Data Center / Server Integration Setup
## Private Packagist Cloud

<div class="row column">
    <div class="callout warning">
        <p>Note: Application Links using OAuth 2 are available since Bitbucket Data Center / Server v7.21. If you are using an older Bitbucket Data Center / Server version then consult the<a href="/docs/cloud/bitbucket-server-oauth1-integration-setup.md">Bitbucket Data Center / Server OAuth 1 setup guide</a>.</p>
    </div>
</div>

## Initial Setup
From the organization settings page, select the "Integrations" subtab.
Hit the "Add Integration" button on the integrations listing page to get to the form below.
To set up a Bitbucket Data Center / Server integration with Private Packagist start by selecting "Bitbucket Data Center / Server"
as the platform, enter the URL of your on-premise Bitbucket Data Center / Server into the base URL field, and select OAuth 2 as OAuth version as seen in the example below.

![Packagist Setup](/Resources/public/img/docs/integration-setup/cloud/bitbucket-server-01-packagist-setup.png)

Copy the content from the "Callback URL / Redirect URL" field and follow the link to set up an Application Link.

## Configure Application link
Click on the link to setup an Application Link on Bitbucket Data Center / Server and select "External application" as type and "Incoming" as direction.

![Bitbucket Create Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-02-bitbucket-create-application-link.png)

After you click the button to continue, enter a name and the content from the "Callback URL / Redirect URL" from the Private Packagist form.
Select Repositories "Read" as application permission and save the form.

![Bitbucket Configure Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-03-bitbucket-configure-application-link.png)

## Finish the Setup

Once the form is saved, Bitbucket Data Center / Server will show you the Client ID and secret.
Copy the them over to Private Packagist and save the integration.

## Configure Bitbucket Server plugins

In case the U2F & TOTP plugin by Alpha Server is installed on the Bitbucket Server then you will need to enable the OAuth whitelist
otherwise Private Packagist will not be able to authenticate with the Bitbucket Server.

![Bitbucket Server TFA Configuration](/Resources/public/img/docs/integration-setup/bitbucket-server-09-tfa.png)
