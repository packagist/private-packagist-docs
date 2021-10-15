# Using Private Packagist in different CI/CD environments

To use Private Packagist in your CI/CD environment, first you'll need to create a read-only authentication token in Private Packagist under *Settings -> Authentication Tokens*. Once the token is created, you'll need to add the `COMPOSER_AUTH` as an environment variable in your CI/CD.
```
COMPOSER_AUTH='{"http-basic": {"repo.packagist.com": {"username": "token", "password": "TOKEN_HERE"}}}'
```

Note that read-only tokens are recommended to use with your CI/CD where you'll be running composer install commands with an existing composer.lock file.

Listed below are some examples for using Private Packagist with different CI/CD environments.

## GitHub Actions

For GitHub actions, you can add the `COMPOSER_AUTH` environment variable at the repository level by going to the repository settings -> secrets -> new repository secret. Environment variables can then be used in your [workflow files](https://docs.github.com/en/actions/learn-github-actions/environment-variables#about-environment-variables).

## Bitbucket Pipeline

You can add the `COMPOSER_AUTH` environment variable at the workspace, repository, and deployment levels on Bitbucket Pipelines as mentioned in [their documentation](https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/).

## Scrutinizer CI

You can add the `COMPOSER_AUTH` environment variable in scrutinizer's website as mentioned in [their documentation](https://scrutinizer-ci.com/docs/build/environment-variables).
