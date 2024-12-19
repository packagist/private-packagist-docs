# GitLab Integration Setup
## Private Packagist Self-Hosted

<div class="row column">
    <div class="callout success">
        <p>This guide explains how to set up an OAuth integration for Private Packagist Self-Hosted with either the on-premises version of GitLab, or their public service on GitLab.com.</p>
        <p>If you are using our Cloud product at packagist.com, <a href="/docs/cloud/gitlab-integration-setup">use this guide</a>.</p>
    </div>
</div>

To allow your users to authenticate to Private Packagist Self-Hosted with their GitLab account, you'll
first need to create an integration in Private Packagist. This document walks you through the required steps.

## Open the Add Integration form
Go to the Admin section and hit the “Add Integration“ button to open the form to create your integration. If you've just
installed Private Packagist Self-Hosted and it is still running in Setup Mode, you do not need to log in. If the application
is no longer in Setup mode, you will have to log in with an admin account first.

![Add Integration](/Resources/public/img/docs/self-hosted/08-integration-create.png)

To set up a GitLab integration with Private Packagist start by selecting "GitLab" as the platform and enter the URL of 
your own GitLab server or use <i>https://gitlab.com</i> for the public GitLab server as seen in the example below. 
A link to set up the OAuth application on GitLab will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/self-hosted/gitlab-01-packagist-setup.png)

## Create a GitLab OAuth Application
Do not submit the integration form yet, but copy the content from the "Callback URL / Redirect URL" and follow the 
setup link to your GitLab server. The fields Client ID and Client Secret remain empty for now.

![GitLab Form](/Resources/public/img/docs/integration-setup/self-hosted/gitlab-02-gitlab-form.png)

Make sure the `api` and `read_user` scopes are both checked and save the new application. This will reveal the credentials required to finish the setup on Private Packagist.

Instead of the user settings, you can also create an application in the settings section of a GitLab group, or if you
have full admin access on GitLab in the GitLab admin section under Applications.
There are two additional checkmarks on that page: `trusted` should remain unchecked, and `confidential` should remain checked.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/self-hosted/gitlab-03-gitlab-credentials.png)

## Create the integration
Copy and paste the "Application Id" and "Secret" value back into the Private Packagist integration form and submit the form with the "Create Integration" button.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/self-hosted/gitlab-04-packagist-form.png)

The GitLab integration will be created and you will be redirected to the admin page.

The new integration will be shown in the list of available integrations, and your users can
now log in to your Private Packagist Self-Hosted installation using their GitLab account.

![Available integrations](/Resources/public/img/docs/integration-setup/self-hosted/gitlab-05-integrations-overview.png)
