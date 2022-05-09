# GitHub (Enterprise) Integration Setup
## Private Packagist Cloud

## Initial Setup

From the organization settings page, select the "Integrations" subtab. Hit the "Add Integration" button on the integrations listing page to get to the form below.
To set up a GitHub Enterprise integration with Private Packagist start by selecting "GitHub" as the platform and enter the URL of your GitHub Enterprise server, as seen in the example below.
A link to setup the OAuth application on your GitHub enterprise server will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/cloud/github-enterprise-01-packagist-setup.png)

## Creating an OAuth Application

Do not submit the integration form yet, but copy the content from the "Callback
URL / Redirect URL" and follow the setup link to your GitHub server. The fields
Client ID and Client Secret remain empty for now.

![GitHub Register App](/Resources/public/img/docs/integration-setup/github-02-github-register-app.png)

Register a new application on GitHub. This will reveal the client ID and client
secret required for finishing the setup on Private Packagist.

![GitHub Credentials](/Resources/public/img/docs/integration-setup/github-03-github-credentials.png)

## Finish the Setup

Copy and paste the "Client ID" and "Client Secret" values back into the Private
Packagist integration form and submit the form with the "Save Integration"
button.

![Packagist Form](/Resources/public/img/docs/integration-setup/cloud/github-enterprise-04-packagist-form.png)
