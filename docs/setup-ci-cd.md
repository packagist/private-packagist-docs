# Using Private Packagist in a CI/CD environment

You can use read-only tokens to grant automated systems like continuous integration or deployment environments access to Private Packagist.

## Adding a read-only token to your organization

You can add read-only tokens on your organization's settings page under _Authentication Tokens_. Make sure to select _Read-only access to packages_ on the access dropdown.

Once the token is created, you'll need to add `COMPOSER_AUTH` as an environment variable in your CI/CD.
```
COMPOSER_AUTH='{"http-basic": {"repo.packagist.com": {"username": "token", "password": "TOKEN_HERE"}}}'
```

Note that read-only tokens are recommended for CI/CD where you'll be running composer install commands with an existing composer.lock file. Read-only tokens are not suitable to run `composer update` as they do not create new mirrored packages. In case you have automated tasks that run package updates, make sure to use tokens with update access.
Read-only tokens don't count as users in your Private Packagist organization, while update tokens will be billed like an additional user account. Read-only tokens can only access packages in your organization. Read-only tokens created in a suborganization will only access packages in the respective suborganization.

## Instructions for CI/CD services

Listed below are some examples for using Private Packagist with different CI/CD environments.

### GitHub Actions

For GitHub actions, you can add the `COMPOSER_AUTH` environment variable at the repository level by going to _repository settings > secrets > new repository secret_. Environment variables can then be used in your [workflow files](https://docs.github.com/en/actions/learn-github-actions/environment-variables#about-environment-variables) to run `composer install`:

```yaml
- name: Install Composer dependencies
  run: composer install --no-progress --prefer-dist --no-dev --no-scripts
  env:
      COMPOSER_AUTH: ${{ secrets.COMPOSER_AUTH }}
```

If you are using [shivammathur/setup-php](https://github.com/shivammathur/setup-php), add the `PACKAGIST_TOKEN` environment variable containing your read-only token:

```yaml
- name: Setup PHP
  uses: shivammathur/setup-php@v2
  with:
    php-version: '8.5'
  env:
    PACKAGIST_TOKEN: ${{ secrets.PACKAGIST_TOKEN }}
```

Please refer to the [shivammathur/setup-php documentation](https://github.com/shivammathur/setup-php?tab=readme-ov-file#github-composer-authentication) for more details. 

### Bitbucket Pipeline

You can add the `COMPOSER_AUTH` environment variable at the workspace, repository, and deployment levels on Bitbucket Pipelines as mentioned in [their documentation](https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/).

### Scrutinizer CI

You can add the `COMPOSER_AUTH` environment variable in scrutinizer's website as mentioned in [their documentation](https://scrutinizer-ci.com/docs/build/environment-variables).
