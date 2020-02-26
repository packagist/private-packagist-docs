# Bitbucket Server (Stash) Integration Setup
## Private Packagist Enterprise

## Initial Setup
Hit the “Add integration“ button on the admin page to get to the form below. To setup a Bitbucket Server (Stash) integration with Private Packagist start by selecting "Bitbucket Server / Stash" as the platform and enter the URL of your on-premise Bitbucket Server into the base URL field as seen in the example below.

![Packagist Setup](/Resources/public/img/docs/integration-setup/bitbucket-server-01-packagist-setup.png)

Submit the form to see the additional information.

![Packagist Form](/Resources/public/img/docs/integration-setup/bitbucket-server-02-packagist-form.png)

## Configure Application link
Click on the link to setup an Application Link on Bitbucket Server / Stash. You will need the "Client Id" and the "Public Key" shown in the form.

![Bitbucket Configure Application Link](/Resources/public/img/docs/integration-setup/bitbucket-server-03-bitbucket-configure-application-link.png)

After you hit the button to create a new Application Link a configuration window may appear. If it does, verify that the url matches your Private Packagist URL and hit "Continue", otherwise skip this step.

![Bitbucket Invalid Url](/Resources/public/img/docs/integration-setup/bitbucket-server-04-bitbucket-invalid-url.png)

Now setup a "Generic Application". The only field required is the "Application Name". Submit the form to finish creating the Application Link.

![Bitbucket Setup Link](/Resources/public/img/docs/integration-setup/bitbucket-server-05-bitbucket-setup-link.png)

![Bitbucket Application Created](/Resources/public/img/docs/integration-setup/bitbucket-server-06-bitbucket-application-created.png)

Click on the pen icon to the right of the application you just created to edit the Application Link and configure Incoming Authentication.

![Bitbucket Incoming Auth](/Resources/public/img/docs/integration-setup/bitbucket-server-07-bitbucket-incoming-auth.png)

This is where we will need the "Client ID" and the "Public Key" that were previously generated on the Private Packagist integration form. Make sure the entire content of the "Public Key" field gets copied and the "Consumer Callback" field stays empty. Submit the form and go back to Private Packagist.

## Finish the Setup
Update the Integration on Private Packagist to save everything and finish the setup.

![Packagist Finalize](/Resources/public/img/docs/integration-setup/bitbucket-server-08-packagist-finalize.png)
