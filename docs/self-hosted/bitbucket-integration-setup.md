# Bitbucket Integration Setup
## Private Packagist Self-Hosted

<div class="row column">
    <div class="callout warning">
        <p>Note: The instructions on this page create an integration with the public Bitbucket at bitbucket.org. If you are trying to setup an integration with your own Bitbucket Data Center / Server consult the <a href="/docs/self-hosted/bitbucket-server-integration-setup.md">Bitbucket Data Center / Server setup guide</a>.</p>
    </div>
</div>

## Initial Setup
Hit the “Add Integration“ button on the admin page to get to the form below.

![Add Integration](/Resources/public/img/docs/self-hosted/08-integration.png)

To setup a Bitbucket integration with Private Packagist start by selecting "Bitbucket" as the platform and enter <i>https://bitbucket.org</i> as the base URL, as seen in the example below.

![Packagist Setup](/Resources/public/img/docs/integration-setup/bitbucket-01-packagist-setup.png)

## Creating an OAuth Application
Do not submit the integration form yet, but copy the content from the "Callback URL / Redirect URL" and go to <a href="https://bitbucket.org/account">https://bitbucket.org/account</a>. Find the "OAuth" menu item under "Access Management" and click on "Add consumer".

![Bitbucket Form](/Resources/public/img/docs/integration-setup/bitbucket-02-bitbucket-form.png)

Make sure all the scopes listed on the Private Packagist form are checked and save the new consumer.

![Bitbucket Form](/Resources/public/img/docs/integration-setup/bitbucket-03-bitbucket-reveal-key.png)

Click on the consumer you just created to reveal the credentials required to finish the setup on Private Packagist.

## Finish the Setup
Copy and paste the "Key" and "Secret" values back into the Private Packagist integration form and submit the form with the "Create Integration" button.

![Bitbucket Form](/Resources/public/img/docs/integration-setup/bitbucket-04-packagist-form.png)
