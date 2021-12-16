# Using Private Packagist in a CI/CD environment

You can use read-only tokens to grant automated systems like continuous integration or deployment environments access to Private Packagist.

## Adding a read-only token to your organization

You can add read-only tokens on your organization's settings page under *Authentication Tokens*. Make sure to select "Read-only access to packages" on the access dropdown.

Once the token is created, you'll need to add the `COMPOSER_AUTH` as an environment variable in your CI/CD.
```
COMPOSER_AUTH='{"http-basic": {"repo.packagist.com": {"username": "token", "password": "TOKEN_HERE"}}}'
```

Note that read-only tokens are recommended to use with your CI/CD where you'll be running composer install commands with an existing composer.lock file. Read-only tokens are not suitable to run `composer update` as they do not create new mirrored public packages. In case you have automated tasks that run package updates, make sure to use tokens with update access or your personal user credentials.
Read-only tokens don't count as users in your Private Packagist account. They can only access packages in your organization. Read-access tokens created in a subrepository will only access packages in that subrepository.

## Examples with some CI/CD environments

Listed below are some examples for using Private Packagist with different CI/CD environments.

### GitHub Actions

For GitHub actions, you can add the `COMPOSER_AUTH` environment variable at the repository level by going to the repository settings -> secrets -> new repository secret. Environment variables can then be used in your [workflow files](https://docs.github.com/en/actions/learn-github-actions/environment-variables#about-environment-variables).

### Bitbucket Pipeline

You can add the `COMPOSER_AUTH` environment variable at the workspace, repository, and deployment levels on Bitbucket Pipelines as mentioned in [their documentation](https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/).

### Scrutinizer CI

You can add the `COMPOSER_AUTH` environment variable in scrutinizer's website as mentioned in [their documentation](https://scrutinizer-ci.com/docs/build/environment-variables).
