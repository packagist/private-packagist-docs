# GitHub Enterprise Integration Setup
## Private Packagist Cloud

<div class="row column">
    <div class="callout success">
        <p>
            This guide explains how to set up an OAuth integration for Private Packagist Cloud with GitHub Enterprise Cloud/Server.
            If you use their public service on GitHub.com, these instructions are not relevant to you. 
        </p>
        <p>If you are using our Self-Hosted product, please <a href="/docs/self-hosted/github-integration-setup">use this guide</a>.</p>
    </div>
</div>

To allow your users to authenticate to Private Packagist with their GitHub Enterprise account, you'll first need to create 
an integration in Private Packagist. This document walks you through the required steps. 

## Open the Add Integration form

From the organization settings page, select the "Integrations" subtab. Hit the "Add Integration" button on the integrations listing page to get to the form below.
To set up a GitHub Enterprise integration with Private Packagist start by selecting "GitHub" as the platform and enter the URL of your GitHub Enterprise server, as seen in the example below.
A link to create a new OAuth application on your GitHub Enterprise server will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/cloud/github-enterprise-01-packagist-setup-20241219.png)

## Create a GitHub Enterprise OAuth Application

Do not submit the integration form yet, but copy the content from the "Callback
URL / Redirect URL" and follow the setup link to your GitHub server. The fields
Client ID and Client Secret remain empty for now.

![GitHub Register App](/Resources/public/img/docs/integration-setup/cloud/github-enterprise-02-register-app.png)

Register a new application on GitHub Enterprise. You'll be redirected to the application's page. Click the "Generate a new client secret" button 
to get a new secret. You'll need the client ID and client secret to finish the setup on Private Packagist.

![GitHub Credentials](/Resources/public/img/docs/integration-setup/github-03-github-credentials-20241219.png)

## Create the integration

Copy and paste the "Client ID" and "Client Secret" values back into the Private
Packagist integration form and submit the form with the "Save Integration"
button.

![Complete integration form](/Resources/public/img/docs/integration-setup/cloud/github-enterprise-04-complete-form.png)

You'll be redirected back to the list of integrations.

### Share the GitHub Enterprise login link

The final step is sharing your organization-specific login link with your users. Look for the "Login link" button next to 
your newly created GitHub Enterprise integration on the integrations page, and copy the URL.

![GitHub Enterprise integration](/Resources/public/img/docs/integration-setup/cloud/github-enterprise-05-integration.png)

This link presents the option to authenticate with your GitHub Enterprise account and can now be used to log in to Private Packagist. 
