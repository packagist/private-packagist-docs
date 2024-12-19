# GitHub (Enterprise) Integration Setup
## Private Packagist Self-Hosted

<div class="row column">
    <div class="callout success">
        <p>This guide explains how to set up an OAuth integration for Private Packagist Self-Hosted with either the on-premises version of GitHub, or their public service on GitHub.com.</p>
        <p>If you are using our Cloud product at packagist.com, <a href="/docs/cloud/github-integration-setup">use this guide</a>.</p>
    </div>
</div>

To allow your users to authenticate to Private Packagist Self-Hosted with their GitHub or GitHub Enterprise account, you'll 
first need to create an integration in Private Packagist. This document walks you through the required steps.

## Open the Add Integration form

Go to the Admin section and hit the “Add Integration“ button to open the form to create your integration. If you've just 
installed Private Packagist Self-Hosted and it is still running in Setup Mode, you do not need to log in. If the application
is no longer in Setup mode, you will have to log in with an admin account first. 

![Add Integration](/Resources/public/img/docs/self-hosted/08-integration-create.png)

To setup a GitHub integration with Private Packagist start by selecting "GitHub" as the platform and enter the URL of 
your GitHub Enterprise Server or <i>https://github.com</i> to use the public GitHub server as seen in the example below. 
A link to set up the OAuth application on GitHub will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/self-hosted/github-01-packagist-setup.png)

## Create a GitHub OAuth Application
Do not submit the integration form yet, but copy the content from the "Callback 
URL / Redirect URL" and follow the setup link to your GitHub server. The fields 
Client ID and Client Secret remain empty for now.

![GitHub Register App](/Resources/public/img/docs/integration-setup/self-hosted/github-02-github-register-app.png)

Register a new application on GitHub Enterprise. You'll be redirected to the application's page. Click the "Generate a new client secret" button
to get a new secret. You'll need the client ID and client secret to finish the setup on Private Packagist.

![GitHub Credentials](/Resources/public/img/docs/integration-setup/github-03-github-credentials-20241219.png)

## Create the integration
Copy and paste the "Client ID" and "Client Secret" values back into the Private 
Packagist integration form and submit the form with the "Create Integration" 
button.

![Private Packagist Integration Form](/Resources/public/img/docs/integration-setup/self-hosted/github-04-packagist-form.png)

The GitHub integration will be created and you will be redirected to the admin page. 

The new integration will be shown in the list of available integrations, and your users can 
now log in to your Private Packagist Self-Hosted installation using their GitHub account.

![Available integrations](/Resources/public/img/docs/integration-setup/self-hosted/github-05-integrations-overview.png)
