# GitHub (Enterprise) Integration Setup
## Private Packagist Enterprise

## Initial Setup
Hit the “Add integration“ button on the admin page to get to the form below. To setup a GitHub integration with Private Packagist start by selecting "GitHub" as the platform and enter the URL of your GitHub Enterprise Server or <i>https://github.com</i> to use the public GitHub server as seen in the example below. A link to setup the OAuth application on GitHub will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/github-01-packagist-setup.png)

## Creating an OAuth Application
Do not submit the integration form yet, but copy the content from the "Callback URL / Redirect URL" and follow the setup link to your GitHub server. The fields Client ID and Client Secret remain empty for now.

![GitHub Register App](/Resources/public/img/docs/integration-setup/github-02-github-register-app.png)

Register a new application on GitHub. This will reveal the client ID and client secret required for finishing the setup on Private Packagist.

![GitHub Credentials](/Resources/public/img/docs/integration-setup/github-03-github-credentials.png)

## Finish the Setup
Copy and paste the "Client ID" and "Client Secret" values back into the Private Packagist integration form and submit the form with the "Create Integration" button.

![Packagist Form](/Resources/public/img/docs/integration-setup/github-04-packagist-form.png)
