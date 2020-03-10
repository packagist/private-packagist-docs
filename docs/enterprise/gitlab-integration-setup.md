# GitLab Integration Setup
## Private Packagist Enterprise

## Initial Setup
Hit the “Add integration“ button on the admin page to get to the form below. To setup a GitLab integration with Private Packagist start by selecting "GitLab" as the platform and enter the URL of your own GitLab server or use <i>https://gitlab.com</i> for the public GitLab server as seen in the example below. A link to setup the oauth application on GitLab will automatically be displayed.

![Packagist Setup](/Resources/public/img/docs/integration-setup/gitlab-01-packagist-setup.png)

## Add an OAuth Application
Do not submit the integration form yet, but copy the content from the "Callback URL / Redirect URL" and follow the setup link to your GitLab server.

![GitLab Form](/Resources/public/img/docs/integration-setup/gitlab-02-gitlab-form.png)

Make sure the "api" and "read_user" scopes are both checked and save the new application. This will reveal the credentials required to finish the setup on Private Packagist.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/gitlab-03-gitlab-credentials.png)

## Finish the Setup
Copy and paste the "Application Id" and "Secret" value back into the Private Packagist integration form and submit the form with the "Create Integration" button.

![GitLab Credentials](/Resources/public/img/docs/integration-setup/gitlab-04-packagist-form.png)
