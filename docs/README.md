# Private Packagist Documentation
## 

## Quick Start
### Create an organization
Log into Private Packagist and create an organization. If you store your private code in GitHub, GitLab, or Bitbucket use the corresponding button to create an organization synchronized with your GitHub, GitLab or Bitbucket organization.

### Set up credentials

If you did not create a synchronized organization or if you store additional private code elsewhere, go to _Settings > Manage Credentials_. Enter a description, e.g. "Bitbucket API Key" and then pick the correct authentication type depending on where your code is stored.

Alternatively you can copy the SSH public key for your organization on the same page and grant the key access to your repositories.

### Add packages

If your organization is synchronized Private Packagist will have created packages for all of your private repositories containing a composer.json. Otherwise or if you would like to add additional packages you can add them in the _Packages_ section of your organization with the Add button. Follow the instructions to pick the right option for your package.

### Configure your project to use Private Packagist

To access Private Packagist you need to set up composer authentication. Copy the authentication command for your user from [your user auth page](https://packagist.com/profile/auth). Run the command to store your user token on your machine.

To grant an automated process like your continuous integration system access, create an access token for your organization under _Settings > Authentication Tokens_. Then either copy the command and add it to the steps to be executed before running composer commands, or use the instructions for defining an environment variable containing composer authentication settings.

Once you have set up authentication, add the Private Packagist repository to your composer.json and disable packagist.org. You can see how the repositories section should look on the organization _Overview_. If you have any other external repositories make sure to add them to Private Packagist and remove all of them from your composer.json

Run `composer update mirrors` in your project to make sure the lock file is updated to use Private Packagist mirror downloads.

That's it, from now on composer update and install will make use of your Private Packagist repository!
