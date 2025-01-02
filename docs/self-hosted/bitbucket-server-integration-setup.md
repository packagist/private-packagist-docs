# Bitbucket Data Center / Server Integration Setup
## Private Packagist Self-Hosted

<div class="row column">
    <div class="callout success">
        <p>This guide explains how to set up an OAuth integration for Private Packagist Self-Hosted with Bitbucket Data Center / Server.</p>
        <p>If you are using our Cloud product at packagist.com, <a href="/docs/cloud/bitbucket-server-integration-setup.md">use this guide</a>.</p>
    </div>
</div>

<div class="row column">
    <div class="callout warning">
        <p>Note: Application Links using OAuth 2 are available since Bitbucket Data Center / Server v7.21. If you are using an older Bitbucket Data Center / Server version then consult the <a href="/docs/self-hosted/bitbucket-server-oauth1-integration-setup.md">Bitbucket Data Center / Server OAuth 1 setup guide</a>.</p>
    </div>
</div>

To allow your users to authenticate to Private Packagist Self-Hosted with their Bitbucket Data Center / Server account, you'll
first need to create an integration in Private Packagist. This document walks you through the required steps.

## Open the Add Integration form
Go to the Admin section and hit the “Add Integration“ button to open the form to create your integration. If you've just
installed Private Packagist Self-Hosted and it is still running in Setup Mode, you do not need to log in. If the application
is no longer in Setup mode, you will have to log in with an admin account first.

![Add Integration](/Resources/public/img/docs/self-hosted/08-integration-create.png)

To setup a Bitbucket Data Center / Server integration with Private Packagist, start by selecting "Bitbucket Data Center / Server"
as the platform, enter the URL of your on-premise Bitbucket Data Center / Server into the base URL field and select OAuth 2 
as the OAuth version as seen in the example below. A link to set up the OAuth application on your Bitbucket instance will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/self-hosted/bitbucket-server-01-packagist-setup.png)

## Create Application link
Do not submit the integration form yet, but copy the content from the "Callback
URL / Redirect URL" and follow the setup link to your Bitbucket Data Center / Server instance. The fields
Client ID and Client Secret remain empty for now.

In Bitbucket, click on the "Create link" button and select "External application" as type and "Incoming" as direction.

![Bitbucket Data Center / Server Create Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-02-bitbucket-create-application-link.png)

After you click the button to continue, enter a name and the content from the "Callback URL / Redirect URL" from the Private Packagist form.
Select Repositories "Read" as application permission and save the form.

![Bitbucket Data Center / Server Configure Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-03-bitbucket-configure-application-link.png)

Once the form is saved, Bitbucket Data Center / Server will show you the Client ID and secret.

## Finish the Setup
Copy and paste the "Client ID" and "Client Secret" values back into the Private
Packagist integration form and submit the form with the "Create Integration"
button.

![Packagist Finalize](/Resources/public/img/docs/integration-setup/self-hosted/bitbucket-server-04-packagist-finalize.png)

The Bitbucket integration will be created and you will be redirected to the admin page.

The new integration will be shown in the list of available integrations, and your users can
now log in to your Private Packagist Self-Hosted installation using their Bitbucket Data Center / Server account.

![Available integrations](/Resources/public/img/docs/integration-setup/self-hosted/bitbucket-server-05-integrations-overview.png)

## Configure Bitbucket Server plugins

In case the U2F & TOTP plugin by Alpha Server is installed on the Bitbucket Data Center / Server then you will need to enable the OAuth whitelist
otherwise Private Packagist won't be able to authenticate with the Bitbucket Data Center / Server.

![Bitbucket Data Center / Server TFA Configuration](/Resources/public/img/docs/integration-setup/bitbucket-server-09-tfa.png)
