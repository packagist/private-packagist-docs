# GitLab (Self-Managed) Integration Setup
## Private Packagist Cloud

## Initial Setup

From the organization settings page, select the "Integrations" subtab. Hit the "Add Integration" button on the integrations listing page to get to the form below.
To set up a GitLab Self-Managed integration with Private Packagist, start by selecting "GitLab" as the plaform and enter the URL of your own GitLab server. A link to setup the OAuth application on your GitLab server will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/cloud/gitlab-self-managed-01-packagist-setup.png)

## Add an OAuth Application

Do not submit the integration form yet, but copy the content from the "Callback URL / Redirect URL" and follow the setup link to your GitLab server.

![GitLab Form](/Resources/public/img/docs/integration-setup/gitlab-02-gitlab-form.png)

Make sure the "api" and "read_user" scopes are both checked and save the new application. This will reveal the credentials required to finish the setup on Private Packagist.

If you have full admin access on GitLab, then you can also create an application in the GitLab admin section under Applications.
There are two additional checkmarks on that page: "trusted" should remain unchecked, and "confidential" should remain checked.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/gitlab-03-gitlab-credentials.png)

## Finish the Setup

Copy and paste the "Application Id" and "Secret" value back into the Private Packagist integration form and submit the form with the "Save Integration" button.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/cloud/gitlab-self-managed-04-packagist-form.png)
