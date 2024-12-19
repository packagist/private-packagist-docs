# GitLab Integration Setup
## Private Packagist Self-Hosted

This guide explains how to setup an OAuth integration for Private Packagist Self-Hosted with either the on-premises version of GitLab, or their public service on gitlab.com.  
If you are using our cloud product at packagist.com, [use this guide](../cloud/gitlab-integration-setup.md).

## Initial Setup
Hit the "Add Integration" button on the admin page in Private Packagist Self-Hosted to get to the form below. 

![Add Integration](/Resources/public/img/docs/self-hosted/08-integration-create.png)

To setup a GitLab integration with Private Packagist start by selecting "GitLab" as the platform and enter the URL of your own GitLab server or use <i>https://gitlab.com</i> for the public GitLab server as seen in the example below. A link to setup the oauth application on GitLab will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/gitlab-01-packagist-setup.png)

## Add an OAuth Application
Do not submit the integration form yet, but copy the content from the "Callback URL / Redirect URL" and follow the setup link to your GitLab server.

![GitLab Form](/Resources/public/img/docs/integration-setup/gitlab-02-gitlab-form.png)

Make sure the "api" and "read_user" scopes are both checked and save the new application. This will reveal the credentials required to finish the setup on Private Packagist.

Instead of the user settings, you can also create an application in the settings section of a GitLab group, or if you
have full admin access on GitLab in the GitLab admin section under Applications.
There are two additional checkmarks on that page: "trusted" should remain unchecked, and "confidential" should remain checked.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/gitlab-03-gitlab-credentials.png)

## Finish the Setup
Copy and paste the "Application Id" and "Secret" value back into the Private Packagist integration form and submit the form with the "Create Integration" button.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/gitlab-04-packagist-form.png)
