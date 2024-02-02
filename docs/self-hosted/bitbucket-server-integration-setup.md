# Bitbucket Data Center / Server Integration Setup
## Private Packagist Self-Hosted

This guide explains how to setup an OAuth integration for Private Packagist Self-Hosted with Bitbucket Data Center / Server integration.  
If you are using our cloud product at packagist.com, [use this guide](../cloud/bitbucket-server-integration-setup.md).

<div class="row column">
    <div class="callout warning">
        <p>Note: Application Links using OAuth 2 are available since Bitbucket Data Center / Server v7.21. If you are using an older Bitbucket Data Center / Server version then consult the<a href="/docs/self-hosted/bitbucket-server-oauth1-integration-setup.md">Bitbucket Data Center / Server OAuth 1 setup guide</a>.</p>
    </div>
</div>

## Initial Setup
Hit the “Add integration“ button on the admin page to get to the form below.
To setup a Bitbucket Data Center / Server integration with Private Packagist, start by selecting "Bitbucket Data Center / Server"
as the platform, enter the URL of your on-premise Bitbucket Data Center / Server into the base URL field and select OAuth 2 as the OAuth version as seen in the example below.

![Packagist Setup](/Resources/public/img/docs/integration-setup/bitbucket-server-01-packagist-setup.png)

Copy the content from the "Callback URL / Redirect URL" field and follow the link to set up an Application Link.

## Configure Application link
Click on the link to setup an Application Link on Bitbucket Data Center / Server and select "External application" as type and "Incoming" as direction.

![Bitbucket Data Center / Server Create Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-02-bitbucket-create-application-link.png)

After you click the button to continue, enter a name and the content from the "Callback URL / Redirect URL" from the Private Packagist form.
Select Repositories "Read" as application permission and save the form.

![Bitbucket Data Center / Server Configure Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-03-bitbucket-configure-application-link.png)

## Finish the Setup

Once the form is saved, Bitbucket Data Center / Server will show you the Client ID and secret.
Copy them over to Private Packagist and save the integration. 

![Packagist Finalize](/Resources/public/img/docs/integration-setup/bitbucket-server-04-packagist-finalize.png)

## Configure Bitbucket Server plugins

In case the U2F & TOTP plugin by Alpha Server is installed on the Bitbucket Data Center / Server then you will need to enable the OAuth whitelist
otherwise Private Packagist won't be able to authenticate with the Bitbucket Data Center / Server.

![Bitbucket Data Center / Server TFA Configuration](/Resources/public/img/docs/integration-setup/bitbucket-server-09-tfa.png)
