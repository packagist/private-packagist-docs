# Using Private Packagist in a CI/CD environment

You can use read-only tokens to grant automated systems like continuous integration or deployment environments access to Private Packagist.

Note that read-only tokens are not suitable to run `composer update` as they do not create new mirrored public packages. To run updates, make sure to use tokens with update access or your personal user credentials.

## Adding a read-only token to your organization

You can add read-only tokens on your organization's settings page under *Authentication Tokens*. Make sure to select "Read-only access to packages" on the access dropdown.

Note that read-only tokens don't count as users in your Private Packagist account.
